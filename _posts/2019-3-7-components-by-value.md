---
layout: post
title: 'vue的组件间传值'
tags: [code]
---


### 父子组件传值--父传子
当在子组件中要使用到父组件传递过来的数据的时候，在子组件定义一个props属性，来接收这个传递过来的值，这个值可以直接在组件中使用，在父组件中使用子组件的位置，把这个值动态的赋值。
```
// 在子组件中定义一个props属性接收传过来的变量，并且使用
<div class="leftNum">{{count}}</div>
export default {
  props: ['count'],
}
// 在父组件中给传递的变量动态赋值
<template>
  <div id="app">
    <num :count="5"></num>
  </div>
</template>
<script>
import Num from '@/components/Num.vue'
export default {
  components: {
    Num
  },
  name: 'App'
}
</script>
```
### 子传父
在父组件中自定义一个事件，这个事件处理函数用来操作子组件传递过来的值，在子组件值发生改变的方法中，通过this.$emit方法来触发父组件的事件，这个方法的第一个参数是父组件中注册的事件名称，第二个参数是要传递给父组件的值。
```
// 在父组件中定义事件
<div id="app">
    <num @numChange="handleChange" :count="n"></num>
    {{n}}
    <router-view/>
</div>
//在事件处理函数中操作子组件传递的值
methods: {
handleChange (e) {
    this.n = e
}
},
// 在子组件的值改变的方法中触发父组件的事件，第一个参数是父组件中的事件名称，第二个要传递的值。
methods: {
handlePlus () {
    this.num++
    this.$emit('numChange', this.num)
}
}
```
### 不相关的组件之间传值
不相关的组件之间不能像子传父那样直接在子组件里触发父组件的事件，因为二者之间没有联系。可以借助第三个对象Vue来建立二者之间的联系，把A组件的值传给B组件
```js
// 独立js文件新建一个Vue对象
import Vue from 'vue'
const vueObject = new Vue ()
export default vueObject
// 在B组件中引入第三个对象，在created中给这个对象自定义一个事件,事件处理函数接收A组件传过来的值
import vueObject from '@/components/vueObject.js'
export default {
    data () {
        return {
            n : 1
        }
    },
    created () {
        vueObject.$on('change',(num)=>{
            this.n = num
        })
    }
}
// A组件中,引入第三个对象，在事件中触发B组件在这个对象中定义的事件，并且传值
import vueObject from '@/components/vueObject.js'
methods: {
    handleChange () {
      this.num++
      vueObject.$emit('change', this.num)
    }
  }
```