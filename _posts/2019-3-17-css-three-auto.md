---
layout: post
title: '使用css实现三栏自适应布局（两边宽度固定，中间自适应--(圣杯布局、双飞翼布局)）'
tags: [code]
---

### 一：绝对定位方法
思路:左右两边元素相对父元素定位到左右两端，让其脱离文档流，中间的盒子会自动上去呈一行显示，并设置中间元素左右margin值为左右两边盒子的宽度。
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        .box{
            position: relative;
        }
        .left,.right{
            width: 50px;
            height: 50px;
            background-color: pink;
            position: absolute;
            top: 0px;
        }
        .left{
            left:0;
        }
        .right{
            right:0;
        }
        .center{
            height:50px;
            margin:0px 50px;         
            text-align: center;                      
        }
    </style>
</head>
<body>
    <div class="box">
        <div class="left">left</div>
        <div class="center">center</div>
        <div class="right">right</div>
    </div>
</body>
</html>
```
### 二：浮动方法
给左右两个元素设置左右浮动，给中间的元素左右margin值设置左右元素的宽度。要注意的是，center中间元素在html结构中要放在左右元素的后面，这样才能保证右边元素不会被挤换行，因为center是在文档流中的
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        .left,.right{
            width: 50px;
            height: 50px;
            background-color: pink;
        }
        .left{
            float: left;
        }
        .right{
            float: right;
        }
        .center{
            height: 50px;
            margin: 0px 50px;
        }
    </style>
</head>
<body>
    <div class="box">
        <div class="left">left</div>
        <div class="right">right</div>
        <div class="center">center</div>
    </div>
</body>
</html>
```
### 三：flex布局
给中间元素设置flex为1
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        .box{
            height:50px;
            display: flex;
        }
        .left,.right{
            width: 50px;
            background-color: pink;
        }
        .center{
            flex: 1;
        }
    </style>
</head>
<body>
    <div class="box">
        <div class="left">left</div>
        <div class="center">center</div>        
        <div class="right">right</div>
    </div>
</body>
</html>
```
### 四：双飞翼布局
把这个布局想象成一只大鸟，先完成身体部分，然后再把翅膀移动到它应该属于的地方。三个元素全部浮动，把center元素放在html结构的前面，外面嵌套一个盒子，给盒子里面的center设置左右margin保证内容不被遮挡，通过margin值留出左右两边元素的位置，左右两边的元素通过负的margin值设置到各自的位置
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        .wrap{
            width: 100%;
            height: 100px;
            float: left;
        }
        .center{
            height: 100px;
            margin: 0 100px;
            background-color: pink;
        }
        .left,.right{
            width: 100px;
            height:100px;
            background-color: orchid;
            float: left;
        }
        .left{
            margin-left: -100%;
        }
        .right{
            margin-left: -100px;
        }
    </style>
</head>
<body>
    <div class="wrap">
        <div class="center">center</div>
    </div>
    <div class="left">left</div>
    <div class="right">right</div>
</body>
</html>
```
### 五：圣杯布局
圣杯布局的实现思路和双飞翼布局大部分内容相似，也是三个全部浮动，调整左右两边元素的margin的值让它们去到合适的位置，但是差别是在实现中间元素内容不被遮挡上面，圣杯布局是在外面嵌套的元素内设置padding的值，左右留出空间，然后给三个元素分别设置相对定位，定位到左右两边
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        .box{
            padding: 0 200px;
        }
        .center {
            width: 100%;
            height: 100px;
            float: left;
            background-color: olive;
            position: relative;                    
        }
        .left,
        .right {
            width: 200px;
            height: 100px;
            background-color: pink;
            float: left;
            position: relative;        
        }
        .left {
            left:-200px;
            margin-left: -100%;
        }
        .right {
            right:-200px;
            margin-left: -200px;
        }
    </style>
</head>
<body>
    <div class="box">
        <div class="center">center</div>
        <div class="left">left</div>
        <div class="right">right</div>
    </div>
</body>
</html>
```