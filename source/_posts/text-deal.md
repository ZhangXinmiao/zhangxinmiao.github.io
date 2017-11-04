---
title: 文字一行居中 多行左对齐的处理方法
date: 2017-11-03 18:35:26
tags: 踩坑记录
---

### 需求：

标题文字，一行的情况下居中；超出一行则左对齐

如果只用`text-align: center`，是达不到这个效果的。

![0](/text-deal/text-align.png)

#### 解决方案：

给`<p>`元素加上`text-align:le`属性，而`<p>`元素的父元素则加上`text-align:center`属性；

这样，文字为一行的时候会居中，而当文字超过一行，p元素被撑开到父元素的宽度，文字就会左对齐了

#### 效果

![0](/text-deal/left.png)

#### code

```html
<!doctype html>
<html>
	<head>
		<title>dealwithword</title>
		<style>
			.container {
				width: 100px;
				height: 100px;
				background-color: #eef;
				text-align: center;
				margin-bottom: 10px;
			}
			p {
				text-align: left;
			}
		</style>
	</head>
	<body>
		<div class="container">
			居中文字
		</div>
		<div class="container">
			<p>换行需要左对齐</p>
		</div>
	</body>
</html>

```

