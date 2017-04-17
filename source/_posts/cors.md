---
title: 跨域资源共享 CORS
date: 2017-04-17 21:41:05
tags: 同源策略
---

### 同源策略

先明确一下同源策略是什么

> the **same-origin policy** is an important concept in the [web application](https://en.wikipedia.org/wiki/Web_application) [security model](https://en.wikipedia.org/wiki/Computer_security_model). Under the policy, a [web browser](https://en.wikipedia.org/wiki/Web_browser_engine) permits scripts contained in a first web page to access data in a second web page, but only if both web pages have the same *origin*.                                                            
>
> ​                                                                                                                                                           ——wikipedia

同源策略是 web 应用安全模型的一个重要概念，在这样的策略下，***只有在两个页面有着相同的源的时候***浏览器允许第一个页面包含的脚本从第二个网页获取数据。

所谓同源：

- 协议相同
- 域名相同
- 端口相同

如果是非同源的，以下这些在获取时都会受到限制：cookie、LocalStorage、IndexDB、DOM、AJAX

### 跨域资源共享（CORS）

#### 了解 CORS

CORS 可以帮我们避开浏览器的同源策略

满足一下条件的，称之为**简单请求**，反之，则为**非简单请求**

- 请求方法为 GET、POST 或 HEAD
- HTTP的头信息不超出以下几种字段：
  - Accept
  - Accept-Language
  - Content-Language
  - Last-Event-ID
  - Content-Type：只限于三个值`application/x-www-form-urlencode`、`multipart/form-data`、`text/plain`

#### 浏览器对于简单 CORS 请求的处理过程

对于简答请求，浏览器直接在请求头中添加 `Origin` 字段，该字段用来表明这次请求的**源**，服务器就是根据这个值来判断是否允许此次跨域请求

如果`Origin`在服务器允许的范围内，服务器则返回相应，并给相应头添加几个字段

- Access-Control-Allow-Credentials 非必须的，一个布尔值，表示是否允许发送 cookie，如果不允许，直接把该字段去掉即可

  如果场景需要我们的请求带上cookie，就需要服务器指定 `Access-Control-Allow-Credentials`值为 true，发出的 ajax 请求也对应的设置`withCredentials`值为true；fetch 方式对应的属性为`credentials`，该值应设置为'include'。


- Access-Control-Allow-Origin 必须的，值为请求时Origin的值或者*（表示全部的源均被允许）
- Access-Control-Expose-Headers
- Content-Type

#### 非简单 CORS 请求的处理过程

对于非简单的请求，浏览器会先发出一次预请求（preflight） OPTIONS，所以当调试时出现了 OPTION 这种情况时，就要仔细查发出的请求是不是简单请求了。
