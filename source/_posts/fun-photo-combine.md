title: 揭开QQ缩略图与大图不同背后的秘密（附简单合成工具）
date: 2016-09-24
categories:
  - 前端
  - JavaScript
tags:
  - JavaScript
  - canvas
  - ps
---
<img src="http://7xp9v5.com1.z0.glb.clouddn.com/%E4%B8%8B%E8%BD%BD.png" alt="">
<!-- more--> 
不知道你是否看过在QQ中看到这样的图片，没点开时是一张图，点开后瞬间变成另外一张如果没见过的话请看下图，如果想要那张图的话访问下面第二个地址
<img src="http://7xp9v5.com1.z0.glb.clouddn.com/change.gif" alt="">
**效果图地址：**http://7xp9v5.com1.z0.glb.clouddn.com/change.png
（你可以保存到手机发到QQ玩玩）

或者你有没有看过QQ空间有段时间刷屏了很多的
**小图时**
<img src="http://7xp9v5.com1.z0.glb.clouddn.com/kk18.png" alt="">
**大图时**
<img src="http://7xp9v5.com1.z0.glb.clouddn.com/kk17.png" alt="">

你肯定觉得很神奇，然而这是如何做到的呢？
<img src="http://7xp9v5.com1.z0.glb.clouddn.com/zhuangbi.jpg" alt="">
## **原理**
其实原理不是很复杂，有人觉得是两帧的gif，其实不然，背后的秘密是png格式的图片，它背景是透明的，然而在QQ或贴吧某些地方，当它是小图时，背景是白色的，当你点击打开后，背景是黑色，你只要控制这张png在白色背景和黑色背景下显示不同的图像就可以了，如果你还不懂,我分`PS`和`程序`方面分别讲制作方法。
> **先睹为快！合成器访问地址**：[https://bupt-hjm.github.io/fun-photo-combine/](https://bupt-hjm.github.io/fun-photo-combine/)
> **github地址**：[https://github.com/BUPT-HJM/fun-photo-combine](https://github.com/BUPT-HJM/fun-photo-combine)


## **PS部分**
### **1.先准备好两张图片**
> 这里略去了去色的步骤（图像->调整->去色），注意这个步骤是必须的，因为这里图片是黑白，所以这个步骤没有用到。

<img src="http://7xp9v5.com1.z0.glb.clouddn.com/change1.jpg" alt="">
<img src="http://7xp9v5.com1.z0.glb.clouddn.com/change22.jpg" alt="">

准备好两张图片，并在下方间好两个背景测试图层，方便看效果
<img src="http://7xp9v5.com1.z0.glb.clouddn.com/kk2.png" alt="">

### **2.白底显示部分**
对于白底显示的图片，我们就需要把高光（白色）部分（你也可以通过取样颜色取图片的高光点）去掉，以保证在背景为黑色的时候，看不出来这个照片
<img src="http://7xp9v5.com1.z0.glb.clouddn.com/kk.png" alt="">
颜色容差和范围自己来选择，选到合适的范围，对于这张图你也可以选跟我一样的，颜色容差42，色彩范围249，然后点击确定
<img src="http://7xp9v5.com1.z0.glb.clouddn.com/kk3.png" alt="">
按delete删除
<img src="http://7xp9v5.com1.z0.glb.clouddn.com/kk4.png" alt="">
然后将白色背景点亮看一下效果
<img src="http://7xp9v5.com1.z0.glb.clouddn.com/kk5.png" alt="">
把黑色背景点亮看一下效果
<img src="http://7xp9v5.com1.z0.glb.clouddn.com/kk6.png" alt="">
<img src="http://7xp9v5.com1.z0.glb.clouddn.com/xiasi.jpg" alt="">
我们要的效果是在黑色背景的时候不会显示它的，该怎么做呢？
点击下图红色框框部分，调整该图层的亮度和对比度
<img src="http://7xp9v5.com1.z0.glb.clouddn.com/liangdu.png" alt="">
<img src="http://7xp9v5.com1.z0.glb.clouddn.com/kk7.png" alt="">
然后做几层应用到那个图层上，在那个调整图层和那个白底图层之间按住alt就可以把那个调整图层只应用到那个图层上（万分重要），最后做成的效果就是原来那个惊悚的女的消失在黑夜中
<img src="http://7xp9v5.com1.z0.glb.clouddn.com/kk9.png" alt="">
<img src="http://7xp9v5.com1.z0.glb.clouddn.com/kk8.png" alt="">
最后把那个图层透明度调整成50%就可以了
你可以点亮白底和黑底图层测试一下，是不是白底图层点亮时显示，黑底图层看到的是全部黑色，如果达到这一步就完成了。
### 3.**黑底显示部分**
黑底其实和白底的处理过程是大概相同的。我们就需要把黑色部分（你也可以通过取样颜色取图片的黑色）去掉，以保证在背景为白色的时候，看不出来这个照片
则就是色彩范围选择的时候将颜色改成取样颜色，然后取黑色，如下这样
<img src="http://7xp9v5.com1.z0.glb.clouddn.com/kk10.png" alt="">
选中后delete，再跟白底处理的一样看一下黑底和白底的时候怎么显示的
同样的，它在白色背景的时候也是辣么惊悚！！
<img src="http://7xp9v5.com1.z0.glb.clouddn.com/kk11.png" alt="">
<img src="http://7xp9v5.com1.z0.glb.clouddn.com/images.jpg" alt="">
还是一样的做亮度对比度处理
<img src="http://7xp9v5.com1.z0.glb.clouddn.com/kk%2012.png" alt="">
跟上面一样也是做好几层调整图层应用到该图层上，直到基本看不见那个黑人
<img src="http://7xp9v5.com1.z0.glb.clouddn.com/kk13.png" alt="">
最后调整下透明度为50%就行


### **4.效果展示**

我们可以做一下图层合并处理（注意:黑底显示图片必须在白底显示图片的图层下方），将除了背景图以外的图层全合并起来，然后我们分别点亮看看效果
**当白色背景点亮时**
<img src="http://7xp9v5.com1.z0.glb.clouddn.com/kk14.png" alt="">
**当黑色背景点亮时**
<img src="http://7xp9v5.com1.z0.glb.clouddn.com/kk15.png" alt="">
是不是很神奇？
<img src="http://7xp9v5.com1.z0.glb.clouddn.com/jizhi.jpg" alt="">
> 如果还不懂可以直接问我hhh


## **canvas合成器**
原理搞懂了，做一个合成器难度也不会很大了，不过在图像处理上还是需要一些小算法，比如去色和调高亮度这些。
> **合成器访问地址**：https://bupt-hjm.github.io/fun-photo-combine/[](https://bupt-hjm.github.io/fun-photo-combine/)
> **github地址**：[https://github.com/BUPT-HJM/fun-photo-combine](https://github.com/BUPT-HJM/fun-photo-combine)

### **类似PS图层**
这个合成器我是用两个canvas来做的，一个上传图片显示的canvas，一个背景canvas，绝对定位到同一地方，实现类似PS图层的效果。
### **ImageData**
我们需要将上传图片的ImageData拿出来做处理。
**1.去色**
其实去色并不难，我从网上找到的算法，就是将r，g，b全部设为图像的亮度就好了
> **亮度公式：**0.299 * red + 0.587 * green + 0.114 * blue

**2.色彩范围处理**
对于白底所要展示的图片，我们需要将高光部分去掉，我们可以对每个像素点做亮度的判断然后根据亮度来做透明度的处理就可以实现效果。
``` javascript
//白底呈现的图片处理
function processing(canvas) {
    var ctx = canvas.getContext("2d");
    var imgData = ctx.createImageData(canvas.width, canvas.height);
    var arr = imgData.data;

    for (var i = 0, len = data.length; i < len; i += 4) {
        var red = data[i],
            green = data[i + 1],
            blue = data[i + 2],
            alpha = data[i + 3],
            y = 0.299 * red + 0.587 * green + 0.114 * blue; //亮度
        var k = 130;
        arr[i] = y;
        arr[i + 1] = y;
        arr[i + 2] = y;
        arr[i + 3] = alpha * (k - y) / 255;
        if (y > k) {
            arr[i + 3] = 0;
        }
    }
    return imgData;

}
//黑底呈现的图片处理
function processingBlack(canvas) {
    var ctx = canvas.getContext("2d");
    var imgData = ctx.createImageData(canvas.width, canvas.height);
    var arr = imgData.data;

    for (var i = 0, len = data.length; i < len; i += 4) {
        var red = data[i],
            green = data[i + 1],
            blue = data[i + 2],
            alpha = data[i + 3],
            y = 0.299 * red + 0.587 * green + 0.114 * blue; //亮度
        y = y * 4.5;
        var k = 100;
        arr[i] = y;
        arr[i + 1] = y;
        arr[i + 2] = y;
        arr[i + 3] = alpha * y / 255 * 0.08;
        if (y < 150) {
            arr[i + 3] = 0;
        }
    }
    return imgData;

}
```
## **图像保存**
由于没有后台，移动端有些无法直接保存生成的图片
> PC端可以直接右键保存或者点击下载图片
> 移动端可以尝试点击下载图片或者生成图片后长按保存

``` javascript
//下载图片
function saveImg() {
    var imgUrl = canvas_img.toDataURL("image/png").replace("image/png", "image/octet-stream");
    var a = document.createElement('a');
    a.download = "fun.png";//指明以产生后缀
    a.href = canvas_img.toDataURL();
    a.click();
}

//生成图片
function createImg() {
    imgBox.innerHTML = "";
    var img = document.createElement("img");
    img.src = canvas_img.toDataURL("image/png");
    imgBox.appendChild(img);
}

```

## **最后**
谢谢阅读~
欢迎上[https://github.com/BUPT-HJM/fun-photo-combine](https://github.com/BUPT-HJM/fun-photo-combine)给个小star哈哈哈
然后我最近开了个公众号，欢迎关注~推送技术相关或随笔等等hh
**欢迎关注**
<img src="http://7xp9v5.com1.z0.glb.clouddn.com/qrcode_for_gh_f4166e610ee5_344.jpg" alt="">

---

>可自由转载、引用，但需署名作者且注明文章出处。

**点击阅读原文访问合成器**






 