title: python爬虫学习(爬取韩寒博客文章链接)
date: 2016-02-08
categories:
  - python
  - 爬虫
tags:
  - python
---

用Eclipse+Pydev作为开发环境，通过python3的`urllib.request`爬取韩寒新浪博客所有文章链接
<!--more-->
## **分析韩寒博客页面**

<img src="http://7xp9v5.com1.z0.glb.clouddn.com/hanhan.png" alt="">

>审查元素查看文章链接

<img src="http://7xp9v5.com1.z0.glb.clouddn.com/shencha.png" alt="">

>这就是我们要的东西(类似如下）)

```
<a title="" target="_blank" href="http://blog.sina.com.cn/s/blog_4701280b0102eksm.html">2013年09月06日</a>
```

>由于要爬取多页还要分析页与页之间的联系

<img src="http://7xp9v5.com1.z0.glb.clouddn.com/ye.png" alt="">

```
第一页：http://blog.sina.com.cn/s/articlelist_1191258123_0_1.html
第二页：http://blog.sina.com.cn/s/articlelist_1191258123_0_2.html
第三页：http://blog.sina.com.cn/s/articlelist_1191258123_0_3.html
...
第七页：http://blog.sina.com.cn/s/articlelist_1191258123_0_72.html

```

>这也就发现了规律，只需要一个循环加字符串连接就可以解决


## **代码的实现**

```
# coding:utf-8           #指定编码为UTF-8
import urllib.request    #引入urllib.request模块
url=['']*350             #初始化一个可放350个字符串的列表存放连接
page=1                   #初始化为第一页
link=1                   #初始化为第一个链接
i=0                      #初始化链接列表下标
while page<=7:           #循环爬遍7页
    con=urllib.request.urlopen('http://blog.sina.com.cn/s/articlelist_1191258123_0_'+str(page)+'.html').read()        #用引入模块方法读取它页面的html代码
    con=con.decode()                         #转变为UTF-8
    title=con.find(r'<a title')              #找到title位置('r'是防止字符转义)
    href=con.find(r'href=',title)            #从title开始搜,找到href位置
    html=con.find(r'.html',href)             #从href开始搜，找到html位置
    while title!=-1 and href!=-1 and html!=1 and i<350:  #如果找不到title，找不到href，找不到html，并且列表遍历完毕
        url[i]=con[href+6:html+5]            #这个可以通过测试切片得出url
        print(link,'  ',url[i])              #输出url
        title=con.find(r'<a title',html)     #下面的操作就跟上面一样了，就是循环
        href=con.find(r'href=',title) 
        html=con.find(r'.html',href)
        i=i+1
        link=link+1
    else:
        print(page,'find end!')
    page=page+1
else:
    print('all find end')     
j=0
link=1
while j<350 and url[j]!='':                            #重新设置一个循环用于写入文件，通过IO操作将链接保存下来
    content=urllib.request.urlopen(url[j]).read()   
    f=open(r'hanhan/'+url[j][-26:],'wb')
    f.write(content)
    print('downloading',link,'  ',url[j])
    j=j+1
    link=link+1
    f.close()
else:
    print('download article finished')
```

## **代码运行**

<img src="http://7xp9v5.com1.z0.glb.clouddn.com/QQ%E5%9B%BE%E7%89%8720160208172200.png" alt="">
<img src="http://7xp9v5.com1.z0.glb.clouddn.com/QQ%E5%9B%BE%E7%89%8720160208172228.png" alt="">
<img src="http://7xp9v5.com1.z0.glb.clouddn.com/fsdfa.png" alt="">
<img src="http://7xp9v5.com1.z0.glb.clouddn.com/fdsfsdfds.png" alt="">

---

>可自由转载、引用，但需署名作者且注明文章出处。






