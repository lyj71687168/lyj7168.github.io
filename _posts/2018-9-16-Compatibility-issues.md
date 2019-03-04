---
layout: post
title: 'js方面常见的兼容性问题小梳理'
tags: [code]
---



## 兼容问题1：获取事件对象

+ 标准方式： 事件处理程序函数的第一个参数 e(事件对象)
+ IE低版本方式：window.event
+ 兼容处理：
  ```javascript
  document.onclick = function (e) {
  	var _event = e || window.event;     
  };
  ```

## 兼容问题2：事件对象中的属性和方法

+  **事件对象.target**的兼容问题
   + 标准：事件对象.target
   + IE低版本：事件对象.srcElement;
   + 兼容处理：
    ```javascript
    document.onclick = function (e) {
    	var _event = e || window.event;  
        var _t = _event.target || _event.srcElement;
    };
    ```

+  **事件对象.preventDefault()** 的兼容问题
   + 标准：事件对象.preventDefault()
   + IE低版本：事件对象.returnValue = false;
   + 兼容处理：
    ```javascript
     var link = document.getElementById('link');
     link.onclick = function(e){
        // 获取事件对象
        var _event = e||window.event;
        alert('鼠标右键被点击');
        if(_event.preventDefault!=undefined){ // 检测浏览器是否支持此方法不支持就是undefined
          _e.preventDefault();  // 标准方式阻止浏览器的默认行为
        }else{ // 不支持
          _event.returnValue = false; //IE低版本方式阻止浏览器的默认行为
        }
      }
    ```

+  **事件对象.stopPropagation()**的兼容问题
   + 标准：事件对象.stopPropagation()
   + IE低版本：事件对象.cancelBubble = true
   + 兼容处理：

    ```html
    <div class="box0">
        box0
        <div class="box1">
            box1
            <div class="box2">
                box2
                <div class="box3">box3</div>
            </div>
        </div>
    </div>
    <script>
        var divs = document.getElementsByTagName("div");
        for(var i = 0; i < divs.length; i++){
            divs[i].onclick = function (e) {
                alert(this.className);
                // 获取事件对象
                var _e = window.event || e;
                // 阻止事件冒泡
                if(_e.stopPropagation!=undefined){ // 浏览器是否支持该方法
                    _e.stopPropagation(); // 标准处理方式
                }else{
                    _e.cancelBubble = true;// IE低版本处理方式
                }
            }
        }
    </script>
    ```

## 兼容3：事件监听的注册和移除
+ 注册事件：
  + 标准：
    ```javascript
    /*
    事件目标.addEventListener(事件类型,事件处理程序,是否捕获);
    */
    ```
  + IE低版本：
    ```javascript
    /*
    事件目标.attachEvent(事件类型,事件处理程序);
    */
    ```
  + 兼容处理：
    ```javascript
      function addEvent(node, type, handler){
        if (node.addEventListener) { // 检测浏览器是否支持标准方式
          node.addEventListener(type, handler);
        } else {
          node.attachEvent('on' + type, handler);
        }
      }
    ```
+ 移除事件：
  - 标准：
    ```javascript
    /*
    事件目标.removeEventListener(事件类型,事件处理程序的名称);
    */
    ```
  - IE低版本：
    ```javascript
    /*
    事件目标.detachEvent(事件类型,事件处理程序的名称);
    */
    ```
  - 兼容处理：
    ```javascript
      function removeEvent(node, type, handlerName){
        if (node.removeEventListener) {// 检测浏览器是否支持标准方式
          //支持
          node.removeEventListener(type, handlerName);
        } else {
          // 不支持
          node.detachEvent('on' + type, handlerName);
        }
      }
    ```

