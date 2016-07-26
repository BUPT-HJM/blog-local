title: 一个纯CSS3实现的酷炫翻书效果
date: 2016-07-24
categories:
  - 前端
  - css3
tags:
  - css3
---
<img src="http://7xp9v5.com1.z0.glb.clouddn.com/EF(%7BOK%7DY%7BCZTFZ)E%252)289R.png" alt="">
<!--more-->
**项目地址：**https://github.com/BUPT-HJM/css3-flip-book
**demo地址：**https://bupt-hjm.github.io/css3-flip-book/
欢迎大家的star啦~

## **项目细节**
> 其实项目中的关键在于几个属性，`perspective`和`rotate`

### **perspective**
>属性指定了观察者与z=0平面的距离，使具有三维位置变换的元素产生透视效果。z>0的三维元素比正常大，而z<0时则比正常小，大小程度由该属性的值决定

``` css
body {
            /*perspective 属性指定了观察者与z=0平面的距离，使具有三维位置变换的元素产生透视效果。z>0的三维元素比正常大，而z<0时则比正常小，大小程度由该属性的值决定。默认情况下，消失点位于元素的中心，但是可以通过设置perspective-origin属性来改变其位置。*/
            -webkit-perspective: 1000px;
            -moz-perspective: 1000px;
            -ms-perspective: 1000px;
            perspective: 1000px;
            background-color: #212121;
            font-family: '微软雅黑';
        }
```


### **preserve-3d**
>transform-style属性指定了，该元素的子元素是（看起来）位于三维空间内，还是在该元素所在的平面内被扁平化。

``` css
.preserve-3d {
            /*transform-style属性指定了，该元素的子元素是（看起来）位于三维空间内，还是在该元素所在的平面内被扁平化。*/
            -webkit-transform-style: preserve-3d;
            -moz-transform-style: preserve-3d;
            -ms-transform-style: preserve-3d;
            transform-style: preserve-3d;
        }
```

### **rotate**

> 在这个效果中`rotate`起到了很重要的作用，特别是`rotateY`，沿着Y轴翻转，你所看到的书本的翻页，便是用`animation`的`@keyframe`动画实现`rotateY`的变化,实现翻页效果跟，也要注意到与`transform-origin`的配合，因为它是旋转轴，尤其关键。

### **欢迎踩踏github，给star啦~**
> 其他便是css的布局与html的配合了，想要了解详细的可以去github看源码学习，欢迎指正，记得给star哦~


---

>可自由转载、引用，但需署名作者且注明文章出处。