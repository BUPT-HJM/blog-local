title: 记一次网易前端实习面试
date: 2016-04-20
categories:
  - 前端
  - 面试
tags:
  - interview
---
记一次网易前端实习面试
<!--more-->
>很幸运地能收到网易的面试通知，就毫不犹豫翘了课去面试了hhhh~
三点的面试，因为从来没去过那个中关村西北旺区，吃完饭早早就去了,想象中那里应该是繁华的地方hhhh，到了发现都在建设中，很多还在建设中，看到了网易旁边的百度和搜狐，都是长长的大楼或者是高高的建筑，满满大企业的既视感~一进网易楼就没网= =，在里面也没事干，就呆在外面看看前端的东西准备下，到2点40的时候跟前台说了下，一个网易年轻姐姐就带我上去了~

## **步入正题-笔试**
>本来我以为只有面试的，发现那个姐姐并不是带我去面试的，带我去了个房间，留了两张题目给我，说半小时来说，毫无防备hhh接下来步入正题吧~

### **1.alert(1&&2),alert(1||0)**

>具体我不记得了反正就这两个，我以为考的是纯粹的与运算和或运算，后来发现太天真了

```
alert(1&&2)的结果是2
只要“&&”前面是false，无论“&&”后面是true还是false，结果都将返“&&”前面的值;
只要“&&”前面是true，无论“&&”后面是true还是false，结果都将返“&&”后面的值;

alert(0||1)的结果是1
只要“||”前面为false,不管“||”后面是true还是false，都返回“||”后面的值。
只要“||”前面为true,不管“||”后面是true还是false，都返回“||”前面的值。
```

### **2.mouseenter和mouseover的区别**

>这个之前看了下，大概是答出来了，但可能不够详细吧

```
不论鼠标指针穿过被选元素或其子元素，都会触发 mouseover 事件。对应mouseout
只有在鼠标指针穿过被选元素时，才会触发 mouseenter 事件。对应mouseleave
```


### **3.用正则表达式匹配字符串，以字母开头，后面是数字、字符串或者下划线，长度为9-20**

>看到这题我是崩溃的，因为正则学的不多，但是稍微写了下也差不多只是忘了些

```
var re=new RegExp("^[a-zA-Z][a-zA-Z0-9_]{9,20}$");
```

### **4.js字符串两边截取空白的trim的原型方法的实现**

```
//我的笨方法，当时还想错了一些，回来后实现了一下，思路是这样
String.prototype.trim = function () {
    var arr=this.split("");
    while(1) {
       if(arr[0]==" ") {
           arr.shift();
           continue;
        }
        break;
    }
    while(1){
        if(arr[arr.length-1]==" ") {
            arr.pop();
            continue;
        }
        break;
    }
    return arr.join("");
}
//后来面试官跟我说一句话就解决了，然而我正则都忘了，平时没怎么用
String.prototype.trim = function () {
    return this.replace(/(^\s*)|(\s*$)/g,'');
};
```

### **5.三道判断输出的题都是经典的题**
```
//5.1
var a=4;
function b() {
   a=3;
   console.log(a);
   function a(){};
}
b();
//明显输出是3，因为里面修改了a这个全局变量，那个function a(){}是用来干扰的，虽然函数声明会提升，就被a给覆盖掉了，这是我的理解

//5.2
//不记得具体的就类似如下
var baz=3;
var bazz={
   baz: 2,
   getbaz: function() {
         return this.baz
  }
}
console.log(bazz.getbaz())
var g=bazz.getbaz;
console.log(g());
//第一个输出是2，第二个输出是3，这题考察的就是this的指向，函数作为对象本身属性调用的时候this指向对象，作为普通函数调用的时候就指向全局了

//5.3
var arr=[1,2,3,4,5];
for(var i=0;i<arr.length;i++)
{ 
   arr[i]=function(){alert(i)}
}
arr[3]();
//典型的闭包啊，看都不用看，肯定弹出5啊
```

### **6.写出position不同值和区别**

>突然想到还有inherit，当时忘记了，后来面试的时候又重新问了我一下

1.absolute: 生成绝对定位的元素，相对于 static 定位以外的第一个父元素进行定位。
元素的位置通过 "left", "top", "right" 以及 "bottom" 属性进行规定。(不占位)
2.relative: 生成相对定位的元素，相对于其正常位置进行定位。因此，"left:20" 会向元素的 LEFT 位置添加 20 像素。（占位）
3.static：默认值。没有定位，元素出现在正常的流中（忽略 top, bottom, left, right 或者 z-index 声明）
inherit：规定应该从父元素继承 position 属性的值。
4.fixed：生成绝对定位的元素，相对于浏览器窗口进行定位。元素的位置通过 "left", "top", "right" 以及 "bottom" 属性进行规定。


### **7.写一个div+css布局，左边图片右边文字，文字环绕图片，外面容器固定宽度，文字不固定（这是后来根据面试官描述的，笔试题上只有图我就不放上来了)**

>这道题我没答好，刚开始我不清楚那个文字是要自适应的
>面试官说用p标签包裹文字，我当时就紧张了下，把p标签错当成内联了，然后我再修正，然后加左浮动,然后不行，我就跟面试官说，我以前都直接就一个img它float：left，加文字不加p标签就好了
>然后我回来试一试才发现= =，直接加p标签就可以了啊= =，omg我的错误！！！

### **8.讲述你对reflow和repaint的理解**

>这个真不会了没接触，第一个我猜是重新布局，第二个倒是见过就是重绘，就想到document.write(),这个后来也没再问我了
>查查查

repaint就是重绘，reflow就是回流。repaint主要是针对某一个DOM元素进行的重绘，reflow则是回流，针对整个页面的重排

**严重性：** 

>在性能优先的前提下，性能消耗 reflow大于repaint。

**体现：**

>repaint是某个DOM元素进行重绘；reflow是整个页面进行重排，也就是页面所有DOM元素渲染。

**如何触发：**

>style变动造成repaint和reflow。
>不涉及任何DOM元素的排版问题的变动为repaint，例如元素的color/text-align/text-decoration等等属性的变动。
>除上面所提到的DOM元素style的修改基本为reflow。例如元素的任何涉及长、宽、行高、边框、display等style的修改。

**常见触发场景：**

触发repaint：

```
color的修改，如color=#ddd；
text-align的修改，如text-align=center；
a:hover也会造成重绘。
:hover引起的颜色等不导致页面回流的style变动。
```

触发reflow：

```
width/height/border/margin/padding的修改，如width=778px；
动画，:hover等伪类引起的元素表现改动，display=none等造成页面回流；
appendChild等DOM元素操作；
font类style的修改；
background的修改，注意着字面上可能以为是重绘，但是浏览器确实回流了，经过浏览器厂家的优化，部分background的修改只触发repaint，当然IE不用考虑；
scroll页面，这个不可避免；
resize页面，桌面版本的进行浏览器大小的缩放，移动端的话，还没玩过能拖动程序，resize程序窗口大小的多窗口操作系统。
读取元素的属性（这个无法理解，但是技术达人是这么说的，那就把它当做定理吧）：读取元素的某些属性（offsetLeft、offsetTop、offsetHeight、offsetWidth、scrollTop/Left/Width/Height、clientTop/Left/Width/Height、getComputedStyle()、currentStyle(in IE))；
```

**如何避免：**

* 尽可能在DOM末梢通过改变class来修改元素的style属性：尽可能的减少受影响的DOM元素。
* 避免设置多项内联样式：使用常用的class的方式进行设置样式，以避免设置样式时访问DOM的低效率。
* 设置动画元素position属性为fixed或者absolute：由于当前元素从DOM流中独立出来，因此受影响的只有当前元素，元素repaint。
* 牺牲平滑度满足性能：动画精度太强，会造成更多次的repaint/reflow，牺牲精度，能满足性能的损耗，获取性能和平滑度的平衡。
* 避免使用table进行布局：table的每个元素的大小以及内容的改动，都会导致整个table进行重新计算，造成大幅度的repaint或者reflow。改用div则可以进行针对性的repaint和避免不必要的reflow。
* 避免在CSS中使用运算式：学习CSS的时候就知道，这个应该避免，不应该加深到这一层再去了解，因为这个的后果确实非常严重，一旦存在动画性的repaint/reflow，那么每一帧动画都会进行计算，性能消耗不容小觑。

## **面试部分**
>半小时写完笔试后，等待面试，hh中途遇到了北邮的师兄聊了一些nodejs的东西
>步入正题面试

1.什么时候开始学前端
2.如何学前端
3.看过谁的博客
4.开始看我的简历问了，问项目，问webpack/gulp区别，问项目如何实现什么的，再问了笔试题（上面讲过了）
5.等等等都问的项目

>基本也就这些，面试官人挺好的，感觉没什么压力~
>最后也让我过了吧，就后面暑假放假再去联系~说我还得多去看看基础的东西~确实基础还不够扎实哈，不过总的来说，这人生第一次面试还挺顺利的说，也是运气好吧~希望学校早放假能去实习一番~


---

>可自由转载、引用，但需署名作者且注明文章出处。