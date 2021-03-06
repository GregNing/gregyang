---
layout: post
title: '在 AJAX 裡使用 AntiForgeryToken 的處理'
date: 2017-08-30 04:25
comments: true
categories: MVC
tags: Csharp MVC
reference:
  name:
    - reload-antiforgerytoken-after-a-login
    - MVC5-antiforgerytoken-how-to-handle-the-provided-anti-forgery-token-was-meant
    - anti-forgery-token-is-meant-for-user-but-the-current-user-is-username
    - mvc-csrf-ajax-antiforgerytoken
  link:
    - https://stackoverflow.com/questions/16815634/reload-antiforgerytoken-after-a-login
    - https://stackoverflow.com/questions/32653049/mvc5-antiforgerytoken-how-to-handle-the-provided-anti-forgery-token-was-meant
    - https://stackoverflow.com/questions/14970102/anti-forgery-token-is-meant-for-user-but-the-current-user-is-username
    - http://kevintsengtw.blogspot.tw/2013/09/aspnet-mvc-csrf-ajax-antiforgerytoken.html
---
##### 新增CommonRazorFunctions.cshtml
```cs
@functions{
  public static string GetAntiForgeryToken()
  {
    string cookieToken, formToken;
    AntiForgery.GetTokens(null, out cookieToken, out formToken);
    return cookieToken + ":" + formToken;
  }

  public static string GetControllerName()
  {
    string controllerNmae = Convert.ToString(HttpContext.Current.Request.RequestContext.RouteData.Values["Controller"]);
    return controllerNmae;
  }
}
```
##### 使用`token`在ajax裡面
```js
$.ajax({
  url: '@Url.Action("Edit",CommonRazorFunctions.GetControllerName())',
  type: "POST",
  contentType: "application/json; charset=utf-8",
  data: JSON.stringify({ key: key }),
  headers: { 'RequestVerificationToken': '@CommonRazorFunctions.GetAntiForgeryToken()' },
  success: function (result) {
    //success
  },
  error: function (error) {
    //error
  },
});
```
##### 在Controller裡面使用Antify 的驗證
```cs
//在執行前的動作
protected override void OnActionExecuting(ActionExecutingContext filterContext)
{
  base.OnActionExecuting(filterContext);
  validateAJAXAntiForgeryTokenAttribute(filterContext);
  getJSONValidateResult(filterContext);
}

/// <summary>驗證AJAX AntiForgeryTokenA</summary>
/// <param name="filterContext">ActionExecutingContext</param>
private void validateAJAXAntiForgeryTokenAttribute(ActionExecutingContext filterContext)
{
  string cookieToken = string.Empty;
  string formToken = string.Empty;

  //如果不是 ajax submit 就return
  if (!filterContext.HttpContext.Request.IsAjaxRequest())
  {
    return;
  }

  //是否ajax 方式呼叫controller 與 POST 方式呼叫，才進入驗證
  var headers = filterContext.HttpContext.Request.Headers;
  IEnumerable<string> xRequestedWithHeaders = headers.GetValues("X-Requested-With").AsEnumerable();
  if (filterContext.HttpContext.Request.HttpMethod.ToUpper() == "POST")
  {
    string headerValue = xRequestedWithHeaders.FirstOrDefault();
    if (!string.IsNullOrEmpty(headerValue))
    {
      string[] tokenHeaders = headers.GetValues("RequestVerificationToken");
      if (tokenHeaders != null && tokenHeaders.Length > 0)
      {
        string[] tokens = tokenHeaders.First().Split(':');
        if (tokens.Length == 2)
        {
          cookieToken = tokens[0].Trim();
          formToken = tokens[1].Trim();
        }
      }
      //驗證是否有偽造
      AntiForgery.Validate(cookieToken, formToken);
    }
  }
}

private void getJSONValidateResult(ActionExecutingContext filterContext)
{
  if (!filterContext.HttpContext.Request.IsAjaxRequest())
  {
    return;
  }
  var modelState = filterContext.Controller.ViewData.ModelState;
  if (!modelState.IsValid)
  {
    var errorModel = modelState
                    .Where(x => modelState[x.Key].Errors.Count > 0)
                    .Select(x => new
                    {
                      x.Key,
                      errors = modelState[x.Key].Errors.Select(y => y.ErrorMessage)
                    });
    filterContext.Result = new JsonResult()
    {
      Data = errorModel
    };
    filterContext.HttpContext.Response.StatusCode = (int)HttpStatusCode.BadRequest;
  }
}
```
如果出現MVC
```cs
AntiForgeryToken - how to handle “The provided anti-forgery token was meant for user ”“, but the current user is ”xxx“.” exception?
```
類似這種問題