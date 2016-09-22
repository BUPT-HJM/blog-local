title: 基于canvas的十滴水小游戏
date: 2016-09-21
categories:
  - 前端
  - JavaScript
tags:
  - JavaScript
  - canvas
---

<img src="http://7xp9v5.com1.z0.glb.clouddn.com/ten-drop.jpg" alt="">
<!--more-->
**游戏访问地址：**[https://bupt-hjm.github.io/ten-drop/](https://bupt-hjm.github.io/ten-drop/)
**github地址：** [https://github.com/BUPT-HJM/ten-drop](https://github.com/BUPT-HJM/ten-drop)

## **面向对象开发**
本项目由四个对象，也可以说是三个对象来构成，一个是水滴`drop`，一个是水滴的集合`dropCollection`,一个是水滴所在的盘`board`,还有一个是游戏对象`game`。小水滴主要处理水滴的绘制，比如各种形状，颜色，位置等，水滴的集合则用来收集页面的水滴来集中处理，`board`部分主要处理画布的坐标部分，`game`则是游戏的逻辑处理，例如初始化游戏，游戏循环渲染重绘制，还有进入下一关。

## **游戏核心-`generation`（代）**
水滴总共有五种形态，由小到大，最后散开，这些均由`generation`来处理，点击水滴或散开水滴碰触，代增加，游戏重绘渲染，根据不同的`generation`来绘制不同形态的水滴，本游戏中处理`generation`为6时则为空白。

## **localStorage存储**
为了保存最佳成绩，使用本地存储localStorage，利用简单的错误处理，实现保存最佳成绩。
``` javascript
Game.prototype = {
    //...
    setBest: function(level) {
        if (window.localStorage) {
            try {
                localStorage.setItem('bla', 'bla');
            } catch (e) {
                best.innerHTML = "";
                return;
            }
        }

        if (!localStorage.getItem("bestL")) {
            localStorage.setItem('bestL', '1');
        }
        if (level) {
            localStorage.setItem("bestL", level);
        }
        bestLevel.innerHTML = localStorage.getItem("bestL");

    }
}
```

## **游戏规则**
因为只是简单休闲小游戏，每关难度随机，每关增加7滴水滴，上限20滴（游戏tips：在第一关中restart找到最佳剩余水滴，再进入后面的关卡）

## **最后**
enjoy yourself！
>以上只是简单介绍一下，想深入可以直接看github源码，欢迎star和fork!

**github地址：** [https://github.com/BUPT-HJM/ten-drop](https://github.com/BUPT-HJM/ten-drop)



---

>可自由转载、引用，但需署名作者且注明文章出处。






