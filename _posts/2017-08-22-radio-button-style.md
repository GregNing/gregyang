---
layout: post
title: 'Radio Button 樣式 '
date: 2017-08-22 04:03
comments: true
categories: CSS
tags: CSS
reference:
  name:
    - Radio Button Style
  link:
    - https://codepen.io/anon/pen/yojgKo
---

Radio Butto Style 在此紀錄下style樣式
##### region 內容框架設定
```css
.radioContainer {
  display: block;
  position: relative;
  margin: 40px auto;
  height: auto;
  width: 500px;
}
```
##### 框架設定 第一層 可有可無排版所需
```css
.radioContainer ul {
  list-style: none;
  margin: 0;
  padding: 0;
  overflow: auto;
}
```
##### 套用li樣式 將開始影響Radio Button樣式設定
```css
.radioContainer ul li {
  color: #AAAAAA;
  display: block;
  position: relative;
  float: left;
  height: 100px;
}
```
##### 將原先的Radio Button 隱藏起來 使用 新的樣式去做選擇
```css
.radioContainer ul li input[type=radio] {
  position: absolute;
  visibility: hidden;
}
```
##### 改變Radio butto的Label 框架字樣
```css
.radioContainer ul li label {
  display: block;
  position: relative;
  font-weight: 300;
  padding: 25px 25px 25px 60px;
  margin: 10px auto;
  height: 30px;
  z-index: 9;
  cursor: pointer;
  -webkit-transition: all 0.25s linear;
}
```
##### 將滑鼠移動到label label顏色將改變
```css
.radioContainer ul li:hover label {
  color: #FFFFFF;
}
```
##### 將改變Radio button 樣式
```css
.radioContainer ul li .check {
  display: block;
  position: absolute;
  border: 2px solid #AAAAAA;
  border-radius: 100%;
  height: 25px;
  width: 25px;
  top: 30px;
  left: 20px;
  z-index: 5;
  transition: border .25s linear;
  -webkit-transition: border .25s linear;
}
```
##### 將滑鼠移動到Radio Button 才會出現效果
```css
.radioContainer ul li:hover .check {
  border: 2px solid;
}
```
##### 將改變已點選radio butto中心點 圖案
```css
.radioContainer ul li .check::before {
  display: block;
  position: absolute;
  content: '';
  border-radius: 100%;
  height: 15px;
  width: 15px;
  top: 5px;
  left: 5px;
  margin: auto;
  transition: background 0.25s linear;
  -webkit-transition: background 0.25s linear;
}
```
##### 改變點選後的Radio樣式 框架顏色以及背景 點選後想要Radio Button顯示什麼顏色可以在這設定值
```css
input[type=radio]:checked ~ .check {
  border: 2px solid #156168;
}
input[type=radio]:checked ~ .check::before {
  background: #156168;
}
input[type=radio]:checked ~ label {
  color: #156168;
}
```