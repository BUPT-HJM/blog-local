title: 带你玩转css3的3D！
date: 2016-07-26
categories:
  - 前端
  - css3
tags:
  - css3
  - 3d
---
<img src="http://7xp9v5.com1.z0.glb.clouddn.com/2L%5DGM_3%5D32QPLP%7DS(1U%5BQQ9.png" alt="">
<!--more-->
话不多说，先上demo
- **酷炫css3走马灯/正方体动画：** https://bupt-hjm.github.io/css3-3d/
- **github源码地址：**https://github.com/BUPT-HJM/css3-3d
- **酷炫css3翻页动画：** https://bupt-hjm.github.io/css3-flip-book/
- **github源码地址：**https://github.com/BUPT-HJM/css3-flip-book
> 以上均纯css3实现，没有使用任何js代码，希望能得到大家的star啦~

## **css3的3d起步**
要玩转css3的3d，就必须了解几个词汇，便是透视(`perspective`)、旋转(`rotate`)和移动(`translate`)。`透视`即是以现实的视角来看屏幕上的2D事物，从而展现3D的效果。`旋转`则不再是2D平面上的旋转，而是三维坐标系的旋转，就包括X轴，Y轴，Z轴旋转。`平移`同理。

当然用理论来说明，估计你还不明白。下面是3个gif：

- 沿着X轴旋转
<img src="http://7xp9v5.com1.z0.glb.clouddn.com/x.gif" alt="">
- 沿着Y轴旋转
<img src="http://7xp9v5.com1.z0.glb.clouddn.com/y.gif" alt="">
- 沿着Z轴旋转
<img src="http://7xp9v5.com1.z0.glb.clouddn.com/z.gif" alt="">

旋转应该没问题了，那理解平移起来就比较容易了，就是在在X轴、Y轴、z轴移动。

你可能会说透视比较不好理解，下面我介绍一下透视的几个属性。

### **perspective**
`perspective`英文名便是透视，没有这东西就没办法形成3D效果，但是这个东西是怎么让我们浏览器形成3D 效果的呢，可以看下面这张图，其实学过绘画的应该知道透视关系，而这里就是这个道理。
<img src="http://7xp9v5.com1.z0.glb.clouddn.com/image_20160727142650.png" alt="">

但是在css里它是有数值的，例如`perspective: 1000px`这个代表什么呢？我们可以这样理解，如果我们直接眼睛靠着物体看，物体就超大占满我们的视线，我们离它距离越来越大，它在变小，立体感也就出来了是不是，其实这个数值构造了一个我们眼睛离屏幕的距离，也就构造了一个虚拟3D假象。

### **perspective-origin**
由上面我们了解了`perspective`，而加上了这个`origin`是什么，我们前面说的这个是眼睛离物体的距离，而这个就是眼睛的视线，我们的视点的不同位置就决定了我们看到的不同景象，默认是中心，为`perspectice-origin: 50% 50%`,第一个数值是 3D 元素所基于的 X 轴，第二个定义在 y 轴上的位置

>当为元素定义 perspective-origin 属性时，其子元素会获得透视效果，而不是元素本身。必须与 perspective 属性一同使用，而且只影响 3D 转换元素。(W3school)

<img src="http://7xp9v5.com1.z0.glb.clouddn.com/origin.png" alt="">


### **transform-style**
**perspective**又来了，没错，它是css中3D的关键，`transform-style`默认是`flat`，如果你要在元素上视线3D效果的话，就必须用上`transform-style: preserve-3d`,否则就只是平面的变换，而不是3D的变换


## **手把手带你玩转css3-3d**
以上我们稍微了解概念，下面就开始实战吧！
我们看一个效果，是不是很酷炫~
<img src="http://7xp9v5.com1.z0.glb.clouddn.com/3d.gif" alt="">

> 如果图片加载不出来，就直接访问[https://bupt-hjm.github.io/css3-3d/](https://bupt-hjm.github.io/css3-3d/)，觉得可以的话记得给star咯hh

### **第一步：html结构**
很简单的一个容器包裹着一个装了6个`piece`的`piece-box`
``` html
<div class="container">
    <div class="piece-box">
        <div class="piece piece-1"></div>
        <div class="piece piece-2"></div>
        <div class="piece piece-3"></div>
        <div class="piece piece-4"></div>
        <div class="piece piece-5"></div>
        <div class="piece piece-6"></div>
    </div>
</div>
```

### **第二步： 加上必要的3D属性，进入3D世界**
通过上面的讲解，应该对`perspective`比较熟悉了吧,
``` css
    /*容器*/
    .container {
        -webkit-perspective: 1000px;
        -moz-perspective: 1000px;
        -ms-perspective: 1000px;
        perspective: 1000px;
    }
    /*piece盒子*/
     .piece-box {
            perspective-origin: 50% 50%;
            -webkit-transform-style: preserve-3d;
            -moz-transform-style: preserve-3d;
            -ms-transform-style: preserve-3d;
            transform-style: preserve-3d;
    }
```

### **第三步：加入必要的样式**
``` css
        /*容器*/
        .container {
            -webkit-perspective: 1000px;
            -moz-perspective: 1000px;
            -ms-perspective: 1000px;
            perspective: 1000px;
        }
        /*piece盒子*/
        .piece-box {
            position: relative;
            width: 200px;
            height: 200px;
            margin: 300px auto;
            perspective-origin: 50% 50%;
            -webkit-transform-style: preserve-3d;
            -moz-transform-style: preserve-3d;
            -ms-transform-style: preserve-3d;
            transform-style: preserve-3d;
        }
        /*piece通用样式*/
        .piece {
            position: absolute;
            width: 200px;
            height: 200px;
            background: red;
            opacity: 0.5;

        }
        .piece-1 {
            background: #FF6666;

        }
        .piece-2 {
            background: #FFFF00;
        }
        .piece-3 {
            background: #006699;
        }
        .piece-4 {
            background: #009999;
        }
        .piece-5 {
            background: #FF0033;
        }
        .piece-6 {
            background: #FF6600;
        }
```

当然，在你完成这步之后你还是只看到一个正方形，也就是我们的`piece-6`,因为我们的3D`transform`还没开始
<img src="http://7xp9v5.com1.z0.glb.clouddn.com/image_20160727151613.png" alt="">

### **第四步：3D变换来袭**
首先是实现走马灯，我们先不要让它走，先实现灯部分。
如何实现呢？因为要构成一个圆，圆是360度，而我们有6个piece，那么，很容易想到，我们需要把每一个piece以递增60度的方式`rotateY`，就变成如下
<img src="http://7xp9v5.com1.z0.glb.clouddn.com/image_20160727152420.png" alt="">
如何把他们从中心拉开呢？

> 这里我们还要注意一点，在我们使元素绕Y轴旋转以后，其实X轴和Z轴也会跟着旋转，所所以正方体的每个面的垂直线还是Z轴，我们就只需要改变下`translateZ`的值,而当`translateZ`为正的时候，就朝着我们的方向走来，这样就可以拉开了

但是拉开的距离如何衡量？

<img src="http://7xp9v5.com1.z0.glb.clouddn.com/image_20160727152714.jpg" alt="">

是不是一目了然了~

下面我们再修改下css
``` css
        .piece-1 {
            background: #FF6666;
            -webkit-transform: rotateY(0deg) translateZ(173.2px);
            -ms-transform: rotateY(0deg) translateZ(173.2px);
            -o-transform: rotateY(0deg) translateZ(173.2px);
            transform: rotateY(0deg) translateZ(173.2px);

        }
        .piece-2 {
            background: #FFFF00;
            -webkit-transform: rotateY(60deg) translateZ(173.2px);
            -ms-transform: rotateY(60deg) translateZ(173.2px);
            -o-transform: rotateY(60deg) translateZ(173.2px);
            transform: rotateY(60deg) translateZ(173.2px);
        }
        .piece-3 {
            background: #006699;
            -webkit-transform: rotateY(120deg) translateZ(173.2px);
            -ms-transform: rotateY(120deg) translateZ(173.2px);
            -o-transform: rotateY(120deg) translateZ(173.2px);
            transform: rotateY(120deg) translateZ(173.2px);
        }
        .piece-4 {
            background: #009999;
            -webkit-transform: rotateY(180deg) translateZ(173.2px);
            -ms-transform: rotateY(180deg) translateZ(173.2px);
            -o-transform: rotateY(180deg) translateZ(173.2px);
            transform: rotateY(180deg) translateZ(173.2px);
        }
        .piece-5 {
            background: #FF0033;
            -webkit-transform: rotateY(240deg) translateZ(173.2px);
            -ms-transform: rotateY(240deg) translateZ(173.2px);
            -o-transform: rotateY(240deg) translateZ(173.2px);
            transform: rotateY(240deg) translateZ(173.2px);
        }
        .piece-6 {
            background: #FF6600;
            -webkit-transform: rotateY(300deg) translateZ(173.2px);
            -ms-transform: rotateY(300deg) translateZ(173.2px);
            -o-transform: rotateY(300deg) translateZ(173.2px);
            transform: rotateY(300deg) translateZ(173.2px);
        }
```
是不是已经实现了走马灯了？
<img src="http://7xp9v5.com1.z0.glb.clouddn.com/2L%5DGM_3%5D32QPLP%7DS(1U%5BQQ9.png" alt="">

### **第五步：animation让3D动起来**
要实现走马灯动，其实很简单，我们只要在`piece-box`上加上旋转动画就行了,5s完成动画，从0度旋转到360度
``` css
        /*piece盒子*/
        .piece-box {
            position: relative;
            width: 200px;
            height: 200px;
            margin: 300px auto;
            perspective-origin: 50% 50%;
            -webkit-transform-style: preserve-3d;
            -moz-transform-style: preserve-3d;
            -ms-transform-style: preserve-3d;
            transform-style: preserve-3d;
            animation: pieceRotate 5s;
            -moz-animation: pieceRotate 5s; /* Firefox */
            -webkit-animation: pieceRotate 5s; /* Safari and Chrome */
            -o-animation: pieceRotate 5s ; /* Opera */
        }
        /*走马灯动画*/
        @keyframes pieceRotate
        {
        0%   {-webkit-transform: rotateY(0deg);
                -ms-transform: rotateY(0deg);
                -o-transform: rotateY(0deg);
                transform: rotateY(0deg);}
        100% {-webkit-transform: rotateY(360deg);
                -ms-transform: rotateY(360deg);
                -o-transform: rotateY(360deg);
                transform: rotateY(360deg);}
        }
        /* Firefox */
        @-moz-keyframes pieceRotate
        {
        0%   {-webkit-transform: rotateY(0deg);
                -ms-transform: rotateY(0deg);
                -o-transform: rotateY(0deg);
                transform: rotateY(0deg);}
        100% {-webkit-transform: rotateY(360deg);
                -ms-transform: rotateY(360deg);
                -o-transform: rotateY(360deg);
                transform: rotateY(360deg);}
        }
        /* Safari and Chrome */
        @-webkit-keyframes pieceRotate
        {
        0%   {-webkit-transform: rotateY(0deg);
                -ms-transform: rotateY(0deg);
                -o-transform: rotateY(0deg);
                transform: rotateY(0deg);}
        100% {-webkit-transform: rotateY(360deg);
                -ms-transform: rotateY(360deg);
                -o-transform: rotateY(360deg);
                transform: rotateY(360deg);}
        }
        /* Opera */
        @-o-keyframes pieceRotate
        {
        0%   {-webkit-transform: rotateY(0deg);
                -ms-transform: rotateY(0deg);
                -o-transform: rotateY(0deg);
                transform: rotateY(0deg);}
        100% {-webkit-transform: rotateY(360deg);
                -ms-transform: rotateY(360deg);
                -o-transform: rotateY(360deg);
                transform: rotateY(360deg);}
        }
```
这里就不放gif了~hhh是不是实现了酷炫的效果，还没结束哦~更酷炫的正方体组装

> 正方体，其实也不难实现，我这里就不很详细说了，你首先可以想象一个面，然后去拓展其他面如何去实现，比如我们把正方体的前面`translateZ(100px）`让它靠近我们100px,然后后面`translateZ(-100px）`让它远离我们100px,左边是先`translateX(-100px `再`rotateY(90deg)`,右边就是`translateX(100px) `再`rotateY(90deg)`,上面是先`translateY(-100px)`，`rotateX(90deg)`，下面是先`translateY(100px)`，`rotateX(90deg)`，只要我们写进动画，一切就大功告成。

css就为如下，以下就只放`piece1`，其他读者可以自己类比实现，或者看我github的源码
``` css
        .piece-1 {
            background: #FF6666;
            -webkit-transform: rotateY(0deg) translateZ(173.2px);
            -ms-transform: rotateY(0deg) translateZ(173.2px);
            -o-transform: rotateY(0deg) translateZ(173.2px);
            transform: rotateY(0deg) translateZ(173.2px);
            animation: piece1Rotate 5s 5s;
            -moz-animation: piece1Rotate 5s 5s; /* Firefox */
            -webkit-animation: piece1Rotate 5s 5s; /* Safari and Chrome */
            -o-animation: piece1Rotate 5s 5s; /* Opera */
            -webkit-animation-fill-mode: forwards; /* Chrome, Safari, Opera */
             animation-fill-mode: forwards;
        }
       /*front*/
        @keyframes piece1Rotate
        {
        0%   {-webkit-transform: rotateY(0deg) translateZ(173.2px);
            -ms-transform: rotateY(0deg) translateZ(173.2px);
            -o-transform: rotateY(0deg) translateZ(173.2px);
            transform: rotateY(0deg) translateZ(173.2px);}
        100% {-webkit-transform: rotateY(0deg)  translateZ(100px);
            -ms-transform:  rotateY(0deg) translateZ(100px);
            -o-transform: rotateY(0deg)  translateZ(100px);
            transform:  rotateY(0deg) translateZ(100px);}
        }
        /* Firefox */
        @-moz-keyframes piece1Rotate
        {
        0%   {-webkit-transform: rotateY(0deg) translateZ(173.2px);
            -ms-transform: rotateY(0deg) translateZ(173.2px);
            -o-transform: rotateY(0deg) translateZ(173.2px);
            transform: rotateY(0deg) translateZ(173.2px);}
        100% {-webkit-transform: rotateY(0deg)  translateZ(100px);
            -ms-transform:  rotateY(0deg) translateZ(100px);
            -o-transform: rotateY(0deg)  translateZ(100px);
            transform:  rotateY(0deg) translateZ(100px);}
        }
        /* Safari and Chrome */
        @-webkit-keyframes piece1Rotate
        {
        0%   {-webkit-transform: rotateY(0deg) translateZ(173.2px);
            -ms-transform: rotateY(0deg) translateZ(173.2px);
            -o-transform: rotateY(0deg) translateZ(173.2px);
            transform: rotateY(0deg) translateZ(173.2px);}
        100% {-webkit-transform: rotateY(0deg)  translateZ(100px);
            -ms-transform:  rotateY(0deg) translateZ(100px);
            -o-transform: rotateY(0deg)  translateZ(100px);
            transform:  rotateY(0deg) translateZ(100px);}
        }
        /* Opera */
        @-o-keyframes piece1Rotate
        {
        0%   {-webkit-transform: rotateY(0deg) translateZ(173.2px);
            -ms-transform: rotateY(0deg) translateZ(173.2px);
            -o-transform: rotateY(0deg) translateZ(173.2px);
            transform: rotateY(0deg) translateZ(173.2px);}
        100% {-webkit-transform: rotateY(0deg)  translateZ(100px);
            -ms-transform:  rotateY(0deg) translateZ(100px);
            -o-transform: rotateY(0deg)  translateZ(100px);
            transform:  rotateY(0deg) translateZ(100px);}
        }         
```


> 细心的读者可以发现我用了一个`animation-fill-mode: forwards;`,这个其实就是让这些`piece`保持动画最后的效果，也就是正方体的效果，读者可以不加试试看，那还是会恢复原样。

最后就是正方体的旋转了，前面我们的容器已经用过`animation`了,读者可能会想我再加个class放`animaiton`不就行了，hhh，`animaiton`会覆盖掉前面的，那前面的走马灯的动画就没了，所以在html结构中，我再加了一个`box`包裹`piece`, 如下


``` html
<div class="container">
    <div class="piece-box">
        <div class="piece-box2"><!--新加的容器-->
            <div class="piece piece-1"></div>
            <div class="piece piece-2"></div>
            <div class="piece piece-3"></div>
            <div class="piece piece-4"></div>
            <div class="piece piece-5"></div>
            <div class="piece piece-6"></div>
        </div>
    </div>
</div>
```

在动画上我们可以控制正方体动画的延时时间，就是等到正方体组装完成后再开始动画
`animation: boxRotate 5s 10s infinite;`第一个5s是动画时长，第二个10s是延时时间，然后我们让正方体的旋转，绕X轴从0度到360度,绕Y轴也0到Y轴360度。

``` 
        .piece-box2 {
            -webkit-transform-style: preserve-3d;
            -moz-transform-style: preserve-3d;
            -ms-transform-style: preserve-3d;
            transform-style: preserve-3d;
            animation: boxRotate 5s 10s infinite;
            -moz-animation: boxRotate 5s 10s infinite; /* Firefox */
            -webkit-animation: boxRotate 5s 10s infinite; /* Safari and Chrome */
            -o-animation: boxRotate 5s 10s infinite; /* Opera */
        }
        /*正方体旋转动画*/
        @keyframes boxRotate
        {
        0%   {-webkit-transform: rotateX(0deg) rotateY(0deg););
            -ms-transform: rotateX(0deg) rotateY(0deg););
            -o-transform: rotateX(0deg) rotateY(0deg););
            transform: rotateX(0deg) rotateY(0deg););}
        100% {-webkit-transform: rotateX(360deg) rotateY(360deg);
            -ms-transform: rotateX(360deg) rotateY(360deg);
            -o-transform: rotateX(360deg) rotateY(360deg);
            transform: rotateX(360deg) rotateY(360deg);}
        }
        /* Firefox */
        @-moz-keyframes boxRotate
        {
        0%   {-webkit-transform: rotateX(0deg) rotateY(0deg););
            -ms-transform: rotateX(0deg) rotateY(0deg););
            -o-transform: rotateX(0deg) rotateY(0deg););
            transform: rotateX(0deg) rotateY(0deg););}
        100% {-webkit-transform: rotateX(360deg) rotateY(360deg);
            -ms-transform: rotateX(360deg) rotateY(360deg);
            -o-transform: rotateX(360deg) rotateY(360deg);
            transform: rotateX(360deg) rotateY(360deg);}
        }
        /* Safari and Chrome */
        @-webkit-keyframes boxRotate
        {
        0%   {-webkit-transform: rotateX(0deg) rotateY(0deg););
            -ms-transform: rotateX(0deg) rotateY(0deg););
            -o-transform: rotateX(0deg) rotateY(0deg););
            transform: rotateX(0deg) rotateY(0deg););}
        100% {-webkit-transform: rotateX(360deg) rotateY(360deg);
            -ms-transform: rotateX(360deg) rotateY(360deg);
            -o-transform: rotateX(360deg) rotateY(360deg);
            transform: rotateX(360deg) rotateY(360deg);}
        }
        /* Opera */
        @-o-keyframes boxRotate
        {
        0%   {-webkit-transform: rotateX(0deg) rotateY(0deg););
            -ms-transform: rotateX(0deg) rotateY(0deg););
            -o-transform: rotateX(0deg) rotateY(0deg););
            transform: rotateX(0deg) rotateY(0deg););}
        100% {-webkit-transform: rotateX(360deg) rotateY(360deg);
            -ms-transform: rotateX(360deg) rotateY(360deg);
            -o-transform: rotateX(360deg) rotateY(360deg);
            transform: rotateX(360deg) rotateY(360deg);}
        }        
```

最后效果，大功告成！

<img src="http://7xp9v5.com1.z0.glb.clouddn.com/3d.gif" alt="">

## **文末**

你是不是也实现了酷炫的css-3d效果呢？
欢迎踩踏[https://github.com/BUPT-HJM/css3-3d](https://github.com/BUPT-HJM/css3-3d)，记得给star哦~~
也欢迎持续关注我的博客[http://bupt-hjm.github.io/](http://bupt-hjm.github.io/)

## **参考**
> http://www.zhangxinxu.com/wordpress/2012/09/css3-3d-transform-perspective-animate-transition/
我也看了网上众多文章/blog，学习了很多，所以也希望自己写一篇给大家分享~谢谢阅读。


---

>可自由转载、引用，但需署名作者且注明文章出处。