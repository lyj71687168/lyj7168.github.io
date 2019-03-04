---
layout: post
title: '会话控制cookie&session'
tags: [code]
---


cookie和session会话控制，用来解决http协议无记忆，无状态的缺陷
### cookie
cookie:存储在客户端，每个cookie小于4kb,例如淘宝的千人千面，可以设置时间，如果不设置，关闭浏览器即清除cookie。
js设置、读取cookie：
```js
document.cookie = "name:'张三"  //设置
document.cookie   // 读取
// 通常不用前端js手段来设置cookie
```

- cookie的原理
  后台设置了cookie以后，使用浏览器访问设置了cookie的页面，cookie信息会随着响应头返回给浏览器并且保存，如果cookie设置了时间则保存在文件中，没有设置时间保存在内存中，之后每一次访问页面的时候，浏览器都会将cookie数据伴随着请求头发送给服务器。

### session
存储在服务端，安全性有保证，一个项目的任何一个页面设置了session,在其他所有页面都可以读取,关闭浏览器session即消失。在用户登录成功后，把重要信息写在session里面，在其他页面验证是否存在，来判断用户是否登录。

- session原理
当访问了一个设置了session的页面时，服务器会形成一个session-id随着响应头（set cookie）返回给浏览器，保存在cookie中，同时服务器也会生成一个以该session-id为名称的文件用来保存session的信息。之后再访问这个网站的时候，请求头（cookie）中携带session-id来和服务器中的文件对比。若是一致，可以读取服务器中的session中的数据。