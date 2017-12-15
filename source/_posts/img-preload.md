---
title: 图片预加载
date: 2017-11-08 17:22:47
tags: 踩坑记录
---

#### 现象

切换小图片图标时图标时有抖动现象出现

#### 分析原因

在切换图片时，可能因为要切换的图片还未加载完成，导致闪烁

#### 解决方案——图片预加载

```javascript
function preloadImg(url) {
    var img = new Image();
    img.src = url;
    if(img.complete) {

    }
    else {
        img.onload = function() {

        };
    }
}
```
