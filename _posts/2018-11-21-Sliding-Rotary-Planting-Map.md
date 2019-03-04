---
layout: post
title: '用jquery写左右滑动轮播图'
tags: [code]
---


之前写了淡入淡出的轮播图，再写一种左右滑动轮播形式的吧。
### first：布局
轮播图构成：图片轮播区、左右点击按钮，跳转小圆点，根据构成我们来这样进行布局。
```html
<div class="banner">
    <div class="lunboimg">
        <img src="images/banner.jpg" alt="">
        <img src="images/banner1.jpg" alt="">
        <img src="images/banner_bg.jpg" alt="">
        <img src="images/banner-3.jpg" alt="">
    </div>
    <div class="leftbtn">
        <img src="images/arrow-l.png" alt="">
    </div>
    <div class="rightbtn">
        <img src="images/arrow-r.png" alt="">
    </div>
    <ul>
        <li class="active"></li>
        <li></li>
        <li></li>
        <li></li>
    </ul>
</div>
```
### second: 样式
我们把最外层父元素相对定位，里面的img元素全部相对父元素绝对定位到父元素的底下，让它们堆叠在一起。并且让他们消失不见，当然除了第一张。然后再把左右按钮和底下的小圆点定位在合适的位置，
```css
.banner {
    width: 1360px;
    height: 620px;
    position: relative;
    margin: 0 auto;
    overflow: hidden;
}

.banner .lunboimg img {
    height: 620px;
    width: 1360px;
    position: absolute;
    top: 0;
    left: 1360px;
}

.banner .lunboimg img:first-child {
    left: 0px;
}

/*左右按钮定位在轮播区*/

.leftbtn {
    position: absolute;
    left: 10px;
    margin-top: 310px;
    top: -22px;
}

.rightbtn {
    position: absolute;
    right: 10px;
    margin-top: 310px;
    top: -22px;
}

.banner ul {
    position: absolute;
    left: 625px;
    bottom: 50px;
}

.banner ul li {
    float: left;
    width: 16px;
    height: 16px;
    background: lightslategray;
    opacity: 0.7;
    border-radius: 8px;
}

.banner ul li+li {
    margin-left: 10px;
}
.banner ul li.active{
    background: lightseagreen;
    opacity: 0.7;
}
```
### third:JS交互
```js
// 给右侧按钮注册轮播点击事件
var index=0 ;
var n=$('.banner li').index();

$('.rightbtn').click(function () {
    
    $('.lunboimg img').eq(index).stop().animate({
        left: -1360
    }, 1000)
    
    index++;
    if (index >$('.lunboimg img').length-1) {
        index = 0;
    }
    $('.lunboimg img').eq(index).css('left', '1360px');
    $('.lunboimg img').eq(index).stop().animate({
        left: 0
    }, 1000)
    $('.banner li').eq(index).addClass('active').siblings().removeClass('active');

    n = index
})
//左侧按钮轮播事件
$('.leftbtn').click(function () {
    $('.lunboimg img').eq(index).stop().animate({
        left: 1360
    }, 1000);
    
    index--;
    if (index < 0) {
        index = $('.lunboimg img').length - 1;
    }
    $('.lunboimg img').eq(index).css('left', '-1360px');
    $('.lunboimg img').eq(index).stop().animate({
        left: 0
    }, 1000);
    $('.banner li').eq(index).addClass('active').siblings().removeClass('active');
    n = index
})
//li点击轮播事件
$('.banner ul li').click(function(){
    $(this).addClass('active').siblings().removeClass('active');
    index=$(this).index();
    if(index>n){
        $('.lunboimg img').eq(n).animate({
            left:-1360
        },1000);
        $('.lunboimg img').css('left','1360px')
        $('.lunboimg img').eq(index).animate({
            left:0
        },1000);
        n=index;
    }else if(index<n){
        $('.lunboimg img').eq(n).animate({
            left:1360
        },1000);
        $('.lunboimg img').css('left','-1360px') 
        $('.lunboimg img').eq(index).animate({
            left:0
        },1000);
        n=index;
    }
})
```