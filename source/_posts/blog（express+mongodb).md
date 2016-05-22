title: 基于Express+MongoDB搭建的简单功能的博客
date: 2016-01-03
categories:
  - 前端
  - nodejs
tags:
  - nodejs
---
总结一下自己学的，自己也总结一下跟着教程遇到的坑

## **关于环境配置和模块安装**

首先是安装环境，node.js不用说了，还有Express和Mongodb
我在本地电脑Windows跟着教程一切都进行得很顺利，然而在阿里云主机的linux上面在安装express然后`npm start`,一直在报错，报错的是error：缺少XX模块什么的，这就很无奈了，明明应该直接`npm stall`就可以安装所需的模块，但是还是缺少，没办法，好的，它报缺少什么模块你就装什么咯，用`npm install XXX（模块）`一个一个装，虽然很烦，但是也无计可施，我也是一个一个装，最后发现没再报错也是比较欣喜的，学了一个阶段后，发现后面的模块安装都是先在package.json中的dependencies加上所需的模块再`npm install`即可完成
<!--more-->
## **关于运行**

运行express的时候linux和Windows的操作是不一样的，这个可以去express官网查看，但是直接`npm start`都是可行的，比较坑的就是每次我们更新代码后就要手动停止运用并重启，这就比较麻烦了，可以安装一个supervisor模块，`npm install -g supervisor`即可，教程中用了`supervisor app`这对于高版本的express是不可行的，要使用`supervisor .\bin\www`这便可以运行，它可以帮助我们自动重启应用

## **关于MongoDB的启动**

同样的，在每次我们要运行博客的时候就要进入mongodb的目录下的bin文件夹下来启动博客，如若没有启动，博客在运行的时候就会报错，我找到的一个解决方法就是可以创建一个.bat文件放在桌面，然后你把所需的命令放进去，每次要用的时候直接点击这个.bat文件即可，如下就是我的.bat文件放的命令

```
D:
cd /nodestudy/mongodb/bin   
mongod --dbpath=D:/nodestudy/mongodb/bin/blog
```

## **其他的一些坑**
`module.exports=Post;`
`module.exports=app;`
诸如此类的话要记得写，因为这样才可以让这里导出函数接口，让其他地方通过require访问，否则就会报出各种undefined的错误
还有由于教程是出来了一段时间了，MongoDB如果和connect-mongo不匹配的话也会报错，你就要在package.json里面把它们一起更新到新版本，然后`npm install`即可

## **搭建Express+MongoDB的简单博客的过程**

我现在就只实现了简单的注册、登录、发表文章的多人博客的功能
总结一下过程

* 首先是要规划好路由，你需要哪些路由，比如这个多人博客，就需要
/：首页
/login：注册
/reg：登录
/post：发表文章
/logout：登出
相应地就要用诸如`app.get('/'，function(req,res){})`和响应这些的`app.post('/login'，function(req,res){})`来规划，这也就是MVC框架中的C部分，controller部分

* 针对路由和所需的功能，相应地在views下面写ejs模板，这就是MVC框架中的V部分，view部分
* 为了记住用户的登录状态用的是会话（session）机制来记入，也就是在登陆后user存入req.session.user然后在post时把req.session.user给user，模板通过判断user来渲染不同的页面
* 为了实现页面通知也就是注册成功错误还有登录成功错误等等，在这个博客中引入的是flash模块，结合重定向来实现
* 实现post请求就需要创建模型与数据进行交互，这就是MVC框架中的M部分，model部分，在博客中用了两个模型一个是User对象一个是Post对象，User.prototype.save实现用户信息的存储，User.get实现用户信息的读取，Post.get和Post.prototype.save分别实现从数据库获取文章和保存文章到数据库等等

>我就只完成了简单的注册登录发表文章的功能，后面有时间再加上其他的功能

## **实现效果如下**

<img src="http://7xp9v5.com1.z0.glb.clouddn.com/%E6%88%AA%E5%9B%BE.png" alt="">

<img src="http://7xp9v5.com1.z0.glb.clouddn.com/%E6%88%AA%E5%9B%BE%20(2).png" alt="">

<img src="http://7xp9v5.com1.z0.glb.clouddn.com/%E6%88%AA%E5%9B%BE%20(1).png" alt="">

## **教程**
>链接：https://github.com/nswbmw/N-blog

---

>可自由转载、引用，但需署名作者且注明文章出处。