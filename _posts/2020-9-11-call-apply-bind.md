---
layout: post
title: "call apply bind 整理"
tags: [code]
---

- 作用:用来重新定义 this 这个对象

```js
var obj = {
  name: "张三",
  myFun: function (sex) {
    console.log(this.name + this.age + sex);
  },
};
var obj1 = {
  name: "李四",
  age: 18,
};
obj.myFun.call(obj1, "男"); // 李四18男     参数用逗号分隔
obj.myFun.apply(obj1, ["男"]); // 李四18男  参数放在数组
obj.myFun.bind(obj1, "男")(); // 李四18男   参数用逗号分隔  返回的是一个新函数，需要再调用一次
```
