---
title: css实现硬件加速
date: 2017-11-03 18:34:07
tags: 踩坑记录
---

#### 现象

华为手机浏览器渲染页面出现页面模糊现象...

#### 解决方案

触发GPU硬件加速

##### 可触发GPU硬件加速的CSS属性有以下几个

- transform
- opacity
- filter

##### 原理

以上属性使浏览器创建了新的复合图层，图层中的动画由GPU进行预处理并且触发了硬件加速


#### code

```CSS
-webkit-transform: translateZ(0);
-moz-transform: translateZ(0);
-ms-transform: translateZ(0);
-o-transform: translateZ(0);
transform: translateZ(0);
```