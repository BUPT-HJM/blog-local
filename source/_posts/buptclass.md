title: nodejs爬取空闲自习室之全过程记录
date: 2016-05-29
categories:
  - 前端
  - nodejs
  - 爬虫
tags:
  - nodejs 
---
网站访问地址：http://buptclass.com/
网站截图(pc端效果)：
<img src="http://7xp9v5.com1.z0.glb.clouddn.com/FireShot%20Capture%2014%20-%20%E5%8C%97%E9%82%AE%E6%9C%AC%E9%83%A8%E8%87%AA%E4%B9%A0%E5%AE%A4%20-%20http___buptclass.com_.png" alt="">
<!--more-->
网站截图(手机端效果)：
<img src="http://7xp9v5.com1.z0.glb.clouddn.com/mobile.PNG" alt="">
爬虫开源：
https://github.com/BUPT-HJM/buptclass
（欢迎star啦啦啦）


## **项目起源**

小伙伴@Ncer在寒假时写了[爬取教务系统空闲自习室的python版本](https://bbs.byr.cn/#!article/Python/12316?p=1),然后我当时正学爬虫和nodejs，觉得可以试试写个nodejs爬虫版本，然后这个小项目就开始啦


## **项目开发过程**
下面就讲述并小结下开发中遇到的问题。
### **如何实现验证码识别**
这个是第一个需要克服的问题。
然后就尝试去寻找nodejs有没有相关的资源学习，然后就找到了[用node.js实现验证码简单识别](http://think2011.net/2016/01/31/node-ocr/),使用`node-tesseract`和`gm`，先用`gm`对图像进行适当处理，然后用`node-tesseract`进行图像识别

>**Tesseract 开源的 OCR 识别工具**
[Tesseract:安装与命令行使用](http://linusp.github.io/2015/04/17/tesseract-install-usage.html)
[node-tesseract](https://github.com/desmondmorris/node-tesseract)
[tesseract-ocr 学习笔记](http://www.cnblogs.com/chinanetwind/p/3179513.html)
[Tesseract-OCR的下载安装](http://www.51testing.com/html/14/87714-3693118.html)
>**graphicsmagick 图像处理工具**
[gm](https://github.com/aheckmann/gm)
[gm文档](http://aheckmann.github.io/gm/)

由于教务系统的验证码识别比较简单，我就没有通过`tesseract 训练`来提高识别率，我用`gm`去了斑h再调整了一下对比度识别率基本就很高了，然后后面加上了识别失败再次访问换图片识别，100%完成正常登陆。

### **如何获取验证码图片**
本来我以为实现了了解验证码识别后，基本登录教务系统这个关卡没什么问题了，然后发现教务系统那个验证码图片是这样的
```
<img id="vchart" width="59" height="24" src="/validateCodeAction.do?random=" +math.random()="" align="absmiddle">
```
= =我怎么才能知道它的random什么鬼呢，看了network也没有用，也是random
我请教了下@Ncer，才发现其中之道。
>感谢@Ncer耐心指导hhh

最后实现就是，先不设cookie访问访问教务系统登录页面，拿到当前cookie，用这个cookie再去访问图片地址，就能拿到当前页面的验证码图片。

## **如何登录**
大概就是，用`fs`将验证码图片写入本地，再`gm`读取图片，祛斑，调对比度，然后用`tesseract`实现验证码识别，然后我用正则表达式来判断这次识别的合法性，不合法则重来一次，再加上学号和密码post基本100%实现登录
>登录方面用了`superagent`

## **如何爬取空闲教室列表**
就在全校课表中的教室使用情况查询里面来爬取
<img src="http://7xp9v5.com1.z0.glb.clouddn.com/KOGNXIAN.PNG" alt="">
要爬取这个空闲的表，就意味着我得获取到这些空白的td格，并且得获取到相应的日期和对应的教室与对应的节数，这就需要花点时间来思考。
首先，我先把这个请求可以获得到的页面下到本地，然后本地测试爬取单独的一张table，因为发现是不按照规律的，我就用`cheerio`处理了一下页面，让它更加易于我的爬取。
处理完大概是这样的
<img src="http://7xp9v5.com1.z0.glb.clouddn.com/chulikongxian.PNG" alt="">
接下来就是找规律了哈哈，
>爬虫有开源，这部分也写得略傻吧~

爬出来的数据结构没弄得很好，大概如此
```
[
  {
    "date": "星期一(05月09日)",
    "building": "主楼",
    "classNu": "1-2节",
    "classroomNu": "主-324(80)"
  },
  {
    "date": "星期一(05月09日)",
    "building": "主楼",
    "classNu": "3-4节",
    "classroomNu": "主-324(80)"
  },
  {
    "date": "星期一(05月09日)",
    "building": "主楼",
    "classNu": "5-6节",
    "classroomNu": "主-324(80)"
  }
```
>其实爬取的时候中间也存在一个爬取不到的问题，爬取要先访问一个地址作为另外一个地址的跳板才可以正常爬取到数据

## **如何循环爬取多个教学楼空闲教室列表**
总不能一个教学楼写一个函数吧，因为这些请求都是有规律的，想着外面套一个for循环不就完了，这样做发现由于这些操作都是异步的会产生问题，所以就得寻求异步的解决方案了。
花了一点时间了解`promise`后，便打算采用`async`来解决这个问题
>[Async 详解](http://blog.csdn.net/marujunyy/article/details/8695205)
[从Promise的Then说起](http://www.alloyteam.com/2016/03/from-the-promise-then-said-on/)


```
//代码部分大概这样吧
var count1 = 0;
async.whilst(//处理异步
    function() { return count1 <= 5 },
    function(cb) {
        console.log("爬取"+buildings[count1]+"完成");
        spiderGo(count1);
        count1++;
        setTimeout(cb, 2000);
    },
    function(err) {
        console.log("爬取成功")
        //console.log('1.1 err: ', err); // -> undefined
        var byrclass = db.get('byrclass');//数据库处理
        byrclass.insert(study,function(err,doc){
            if (err) console.log(err);
            else console.log("数据库写入成功")
        });
        db.close();
        sync();//upload
        console.log("上传成功");
    }
);
```

## **爬取的数据要放到哪里**
刚开始想着用mongodb，便学习着@piegg的[爬取十大](https://github.com/Molunerfinn/Nodejs-ByrTopTen)跟着写了本地mongodb的存取,使用`monk`和`mongodb`连接数据库并且插入数据库。

然后也写了个`fs`直接写入json文件
>由于后来直接用vue-resource读取，然后就直接用json了，后面会介绍

## **如何展现爬取的数据**
因为也在练手学`vue`,于是就采用了`vue`做前端框架，而且这个也算单页面应用，后面说不定还会加点东西，所以选择了`vue`
然后用到了`vue-resource`来获取json数据，最后data渲染到页面，然后想着要做table过滤，然后发现了`vue`官方示例正好有一个很类似的[表格组件](http://vuejs.org.cn/examples/grid-component.html),于是加以改造，便得到了现在这个页面。
>css部分我就直接用了boostrap.min.css了快捷点，其实那个也不大。

我直接用`webpack`来处理那个require (vue-resource)，因为后面也说不定会加上一些vue组件,也练一下手，就用上了打包成一个bundle.js，页面加载这个就OK。而在使用webpack的过程中发现了`webpack-dev-server`，其实我现在这个页面也没有后台，直接起一个静态资源服务器也未尝不可吧，在云主机用nginx反向代理一下，就完成了。
<img src="http://7xp9v5.com1.z0.glb.clouddn.com/FireShot%20Capture%2014%20-%20%E5%8C%97%E9%82%AE%E6%9C%AC%E9%83%A8%E8%87%AA%E4%B9%A0%E5%AE%A4%20-%20http___buptclass.com_.png" alt="">

## **最后的问题**
本来打算用下`node-schedule`（在@piegg学到的）然后定时爬取
在我把项目文件放到云主机上突然发现= =教务系统是无法外网访问的
所以想着解决方案，omg，访问http://sslvpn.bupt.edu.cn/，这个vpn关闭了？
最后就放弃在云主机上部署爬虫，估计很难行得通，便打算直接在本地上传数据，但是总不能我手动吧= =这也太2了hhh
然后就打算再写一个脚本来实现这些操作，觉得要用node就全用node吧
就发现了`node-ssh`

>github真是天堂，找什么都有
>and node大法好！

然后我看着`node-ssh`只有介绍ssh登录服务器的一些操作，然后想着应该有人封装好了上传文件的东西吧，查了一下还真有，不过star量并不多，叫做[node-ssh2sync](https://github.com/robogeek/node-ssh2sync),依赖于上面这个`node-ssh`，然后在填写了一些配置信息后也成功可以实现上传同步。

then
肯定不能我自己点着脚本再上传啊~接着就把写了个bat文件注册成windows服务，一开机登录网关，登上校园网，爬取数据，上传云主机，大功告成~

###  **待解决的问题**
- 页面上数据冗余量大，有一些不必要的信息存在

>由于页面直接用`vue`的`filterBy`来过滤，再加上爬虫部分其实数据结构也可以改进，这个问题我会想想

- 由于直接用`vue-resource`加载整个`json`，加载速度有待改进-

>这些都会慢慢改进，有什么好建议也可以直接跟我说

**爬虫开源：**
https://github.com/BUPT-HJM/buptclass
（欢迎star啦啦啦）

>本文可能有很多错误欢迎指正啦~
>感谢@Ncer提供域名和指导，还有@piegg的指导

---

>可自由转载、引用，但需署名作者且注明文章出处。