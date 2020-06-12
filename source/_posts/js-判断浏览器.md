---
title: js如何判断不同浏览器
date: 2020-04-06
tags: javascript
---

IE浏览器主要通过这个语句判断
```javascript
if (!!window.ActiveXObject || "ActiveXObject" in window)
```
除IE以外的浏览器都可以通过navigator.userAgent来判断
### edge
```javascript
if (navigator.userAgent.indexOf("Edge") > -1)
```
###Safari
```javascript
if (userAgent.indexOf("Safari") > -1 && userAgent.indexOf("Chrome") < 1)
```
### chrome

```javascript
if ((navigator.userAgent.toLowerCase().match(/chrome\/[\d.]+/gi) + '').replace(/[^0-9.]/ig, "").split('.')[0] == '45')
```
