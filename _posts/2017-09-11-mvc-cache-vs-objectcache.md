---
layout: post
title: 'MVC Cache VS ObjectCache'
date: 2017-09-11 16:53
comments: true
categories: MVC
tags: MVC Csharp
reference:
  name:
    - ObjectCache
    - Cache-API-and-ObjectCache
  link:
    - https://jeffprogrammer.wordpress.com/2015/12/05/caching-in-asp-net-mvc-5/
    - https://blog.miniasp.com/post/2010/05/01/ASPNET-4-Cache-API-and-ObjectCache.aspx
---

##### 快取`API`有兩種`：Cache`與`ObjectCache`
先示範第一種寫法 Cache
```cs
private IEnumerable<testmodel> GettestInfoCache()
{
  Cache Cache = new Cache();
  string cachekey = string.Format("{0}_testinfo", "test");
  IEnumerable<testmodel> testInfoList = (Cache[cachekey] as List<testmodel>);
  onRemove = new CacheItemRemovedCallback(this.RemovedCallback);
  if (commonInfoList == null || commonInfoList.Count() == 0)
  {
    lock (lockObject)
  {
  testInfoList = GetAlltestInfoConnection();
  //使用Cache 取得資料 Cache Insert 會取代資料 Cache Add並不會取代 保險狀況使用 Inserts
  Cache.Insert(cachekey, testInfoList, null, DateTime.Now.AddHours(2), Cache.NoSlidingExpiration, Cache	 	ItemPriority.High, onRemove);}}return testInfoList;}
```

#### 建議使用 ObjectCache
`ASP.NET`的`Cache`與`MemoryCache`雖然都是記憶體快取(In-memory cache)，但在 ASP.NET 中只能使用一份 Cache 物件。<br>
而在`MemoryCache`(ObjectCache 就是使用 MemoryCache)可在AppDomain中建立多份快取物件。




