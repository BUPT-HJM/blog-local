title: JavaScript-弹出层的实现
date: 2016-01-05
categories:
  - 前端
  - JavaScript
tags:
  - JavaScript
---
## **弹出层的实现思路**

>本文讲述的弹出层是点击按钮后弹出两个层，一个是遮罩层,一个是弹出的想让用户看到的层，不能改变页面布局怎么做呢？就需要动态生成这两个层，用js控制它们的出现与消失，控制它们的位置，然后css中写对应的样式代码，这也就是构建弹出层的大体思路

## **遮罩层的实现**

>遮罩层的实现要遮罩住整个页面，这个层就需要有整个页面的高度和宽度

```
var allWidth=document.body.scrollWidth;
var allHeight=document.body.scrollHeight;
```

>scrollHeight: 获取对象的滚动高度。
scrollLeft:设置或获取位于对象左边界和窗口中目前可见内容的最左端之间的距离
scrollTop:设置或获取位于对象最顶端和窗口中可见内容的最顶端之间的距离
scrollWidth:获取对象的滚动宽度
offsetHeight:获取对象相对于版面或由父坐标 offsetParent 属性指定的父坐标的高度
offsetLeft:获取对象相对于版面或由 offsetParent 属性指定的父坐标的计算左侧位置
offsetTop:获取对象相对于版面或由 offsetTop 属性指定的父坐标的计算顶端位置
event.clientX 相对文档的水平座标
event.clientY 相对文档的垂直座标
event.offsetX 相对容器的水平坐标
event.offsetY 相对容器的垂直坐标
document.documentElement.scrollTop 垂直方向滚动的值
event.clientX+document.documentElement.scrollTop 相对文档的水平座标+垂直方向滚动的量
[参考链接](http://blog.csdn.net/xuantian868/article/details/3116442)
<!--more-->
然后生成结点并在页面中生成
```
var oMask=document.createElement("div");
    oMask.id="mask";
    oMask.style.width=allWidth+"px";
    oMask.style.height=allHeight+"px";
    document.body.appendChild(oMask);
```
这也就大致完成了操作，然而遮罩层所需的css样式需要半透明，在css样式中实现
```
#mask {
        background-color: #000;
        opacity: 0.5;
        filter: alpha(opacity=50); 
        position: absolute; 
        left: 0;
        top: 0;
        z-index: 1000;
    }
```
>filer是为了兼容ie

## **遮罩层用户弹出层的实现**

>同样是获取高度和宽度，这次就不加以赘述，略有不同，可以在css中设置好，重点在于定位方面，要定位在页面中央

```
var seeHeight=document.documentElement.clientHeight;
var oalert=document.createElement("div");
    oalert.id="alertsth";
    oalert.innerHTML="<div class='sth'><div id='close'>点击关闭</div></div>";
    document.body.appendChild(oalert);
var sthHeight=oalert.offsetHeight;
var sthWidth=oalert.offsetWidth;
    oalert.style.left=(allWidth-sthWidth)/2+"px";
    oalert.style.top=(seeHeight-sthHeight)/2+"px";
```

## **弹出与关闭的实现**

>页面的js代码

```
window.onload=function(){
    var oBtn=document.getElementById("alertBtn");
    oBtn.onclick=function(){
        opensth();
        return false;//阻止浏览器默认下一步操作
    }
}
function opensth(){
    var allWidth=document.body.scrollWidth;
    var allHeight=document.body.scrollHeight;
    var seeHeight=document.documentElement.clientHeight
    var oMask=document.createElement("div");
        oMask.id="mask";
        oMask.style.width=allWidth+"px";
        oMask.style.height=allHeight+"px";
        document.body.appendChild(oMask);
    var oalert=document.createElement("div");
        oalert.id="alertsth";
        oalert.innerHTML="<div class='sth'><div id='close'>点击关闭</div></div>";
        document.body.appendChild(oalert);
    var sthHeight=oalert.offsetHeight;
    var sthWidth=oalert.offsetWidth;
        oalert.style.left=(allWidth-sthWidth)/2+"px";
        oalert.style.top=(seeHeight-sthHeight)/2+"px";
    var oClose=document.getElementById("close");
    oClose.onclick=oMask.onclick=function(){
        document.body.removeChild(oalert);
        document.body.removeChild(oMask);
    };
```