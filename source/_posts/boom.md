title: 关于一个基于canvas的原生js图片爆炸插件
date: 2016-07-10
categories:
  - 前端
  - JavaScript
tags:
  - JavaScript
  - canvas
---
<img src="http://7xp9v5.com1.z0.glb.clouddn.com/%E5%9B%BE%E5%83%8F%201.jpg" alt="">
<!--more-->
**DEMO访问地址：**https://bupt-hjm.github.io/BoomGo/
**插件及使用方法地址：** https://github.com/BUPT-HJM/BoomGo
**动画：**<img src="http://7xp9v5.com1.z0.glb.clouddn.com/GIF.gif" alt="">

## **1.参考JQuery，支持链式调用**
```
(function(window, undefined) {
  //...
  // A.prototype.init.prototype指向A.prototype
  boom.prototype.init.prototype = boom.prototype;
  //暴露变量
  window.boom = boom;
})(window)
```
## **2.Canvas API `getImageData `对图像颜色采样**
```
  /**
   * 获取canvas像素值,构造colors数组
   * @param   ctx    绘图上下文
   * @param   canvas canvas元素
   * @return  colors colors数组
   */
  function initColors(ctx, canvas) {
    var colors = [];
    var imagedata = ctx.getImageData(0, 0, canvas.width, canvas.height);
    var data = imagedata.data;
    for (var x = 0; x < canvas.width; x++) {
      for (var y = 0; y < canvas.height; y++) {
        //获取进入数组的偏移量
        var i = ((y * canvas.width) + x) * 4;

        var r = data[i];
        var g = data[i + 1];
        var b = data[i + 2];
        var a = data[i + 3];
        var color = {
          r: r,
          g: g,
          b: b,
          a: a
        }
        colors.push(color);
      }
    }
    return colors;
  }
```
## **3.支持自定义爆炸参数**
```
//默认参数
var argOptions = {
    'radius': 10,//小球大小
    'minVx': -30,//最小水平喷射速度
    'maxVx': 30,//最大水平喷射速度
    'minVy': -50,//最小垂直喷射速度
    'maxVy': 50,//最大垂直喷射速度
    'edgeOpacity': false//canvas是否边缘羽化
}
//爆炸go支持延时默认为立即爆炸即go(0)
```

```
//使用自定义参数，可以不写全
var argOptions = {
    'radius': 10,//小球大小
    'minVx': -30,//最小水平喷射速度
    'maxVx': 30,//最大水平喷射速度
    'minVy': -50,//最小垂直喷射速度
    'maxVy': 50,//最大垂直喷射速度
    'edgeOpacity': false//是否canvas边缘羽化
}
boom("canvas1","imgs/test1.jpg",argOptions).go(3000);
//3s后按argOptions参数爆炸id为canvas1的图片
```

## **更多东西由你发现**
>初学canvas，欢迎follow和star，pull request，提出您的宝贵意见
**github地址：** https://github.com/BUPT-HJM/BoomGo

## **感谢**
- 感谢@chokcoco与@xxycode带来的灵感和部分代码参考
- 感谢@kiki9611的建议
- 参考
  - https://github.com/chokcoco/boomJS
  - https://github.com/xxycode/UIViewXXYBoom
---

>可自由转载、引用，但需署名作者且注明文章出处。