title: nodejs学习-网络爬虫与数据操作
date: 2016-02-10
categories:
  - 前端
  - nodejs
  - 爬虫
tags:
  - nodejs
  - mysql
---

跟着《nodejs实战》写了一个爬虫并存入数据库的代码，虽然还有很多地方一知半解，但是也是学到了点东西
<!--more-->

## **如何获取我们想要的数据**

>爬虫的道理都是一样的，nodejs和python一样的道理，引入模块，请求页面，然后提取页面信息，再进行下一步的操作,nodejs是通过`requiest`模块来请求页面，再通过`cheerio`模块来提取页面内容，这个`cheerio`模块支持部分jQuery的操作，比较友好得多

## **爬下来的数据存到哪里**

>nodejs下还有一个`mysql`模块,通过mysql的连接查询插入等操作来存储或调用数据

## **如何显示这些数据**

>那就需要创建web服务器，这里就要用到`express`，通过模板渲染，从数据库中调取数据，便实现了页面

## **最终实现效果**

>单纯实现未加css
<img src="http://7xp9v5.com1.z0.glb.clouddn.com/UC%E6%88%AA%E5%9B%BE20160223112955.png">

## **代码**

>https://github.com/BUPT-HJM/nodejs-study-spider


---

>可自由转载、引用，但需署名作者且注明文章出处。