---
title: 安卓手机line-height文字向上偏移
date: 2017-11-05 00:01:30
tags: 踩坑记录
---
#### 现象

利用line-height属性实现图标的上下居中，IOS系统中浏览器表现正常；而在oppo R9、华为等安卓手机上则会偏上一些

```CSS
height: .28rem;
line-height: .28rem;
```


<img src="android-line-height/before.png" width="20%">

#### 解决方案

这里抛弃了利用line-height实现上下居中的方法，直接将line-height设置为0；利用padding将元素撑开

#### code

```CSS
line-height: 0;
padding: .14rem 0 .14rem 0;
```