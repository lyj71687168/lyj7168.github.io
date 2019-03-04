---
layout: post
title: 'CSS之如何实现一个元素的垂直水平居中'
tags: [code]
---


关于如何实现一个元素的垂直水平居中，是我们比较常见的一个问题，今天我们就常用方法进行一个总结。
### 第一种：margin和transform：translateY结合
也就是给子元素设置margin左右边距为水平居中，上边距为50%，再通过位移向上移动子元素自身高度的一半。
```
<style>
    .box {
        width: 300px;
        height: 300px;
        border: 1px solid #000;
    }
    .content{
        width: 100px;
        height: 100px;
        border: 1px solid #cccccc;
        margin: 50% auto 0;
        transform: translateY(-50%);
    }
</style>

<div class="box">
    <div class="content"></div>
</div>
```
### 第二种&第三种：绝对定位和transform：translateY或者margin结合结合
利用子绝父相，把元素先移动到垂直水平方向的一半处，然后可以用margin向左和向上移动自身的一半，此时margin的值是相对于自己的，不能设置为50%，百分比单位是相对于父元素的，如果在这里设置成百分比，那么就不是移动自身的一半啦，所以这样做的缺点是必须知道元素的宽度和高度。

如果子元素自身的宽度不能被2整除，那么移动的距离就不是很精确。所以还可以用transform位移的方式，来移动自身的50%；缺点是css3的属性只支持IE9+的浏览器
```
.box {
        width: 300px;
        height: 300px;
        border: 1px solid #000;
        position: relative;
    }
    .content{
        width: 100px;
        height: 100px;
        border: 1px solid #cccccc;
        position: absolute;
        top: 50%;
        left: 50%;
        /* margin: -50px 0 0 -50px; */    margin和transform二选一
        transform: translateX(-50%) translateY(-50%)
    }

<div class="box">
    <div class="content"></div>
</div>
```
### 第四种，绝对定位和margin ：auto结合
把子元素绝对定位后上下左右都设置为0；然后margin auto
```
.box {
        width: 300px;
        height: 300px;
        border: 1px solid #000;
        position: relative;
    }
    .content{
        width: 100px;
        height: 100px;
        border: 1px solid #cccccc;
        position: absolute;
        top: 0;
        bottom: 0;
        left: 0;
        right: 0;
        margin: auto;
    }
<div class="box">
    <div class="content"></div>
</div>
```
### 第五种 table-cell布局
把父元素设置display:table,子元素设置display：table-cell(相当于表格里面的td,是行内元素)，只设置这两个，可以用于多行文本的垂直居中，如果里面的元素想要设置宽高，则需要再嵌套一层，并转化成行内块。
```
.box{
        width: 300px;
        height: 300px;
        border: 1px solid #000;
        display: table;
    }
    .content{
        border: 1px solid red;
        display: table-cell;
        text-align: center;
        vertical-align: middle;
    }
    .inner{
        width: 100px;
        height: 100px;
        display: inline-block;
        background-color: pink;
        line-height: 100px;
    }
<div class="box">
    <div class="content">
        <div class="inner">
            你好         
        </div>
    </div>
</div>
```
### 第六种flex布局
```
.box{
        width: 300px;
        height: 300px;
        border: 1px solid #000;
        display: flex;
        justify-content: center;
        align-items: center;
    }
    .content{
        width: 100px;
        height: 100px;
        border: 1px solid red;
    }
<div class="box">
     <div class="content">
     </div>
</div>
```