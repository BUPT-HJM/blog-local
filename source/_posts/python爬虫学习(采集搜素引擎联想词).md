title: python爬虫学习(采集搜素引擎联想词)
date: 2016-02-09
categories:
  - python
  - 爬虫
tags:
  - python
---

以360搜索和百度搜索首页为例用python采集搜索引擎联想词
<!--more-->

## **以360搜索首页为例分析**

>打开www.so.com

<img src="http://7xp9v5.com1.z0.glb.clouddn.com/544.png" alt="">

>右键审查元素，输入关键词，可以看到联想，并查看network

<img src="http://7xp9v5.com1.z0.glb.clouddn.com/4646.png" alt="">

>可以看到它的response

<img src="http://7xp9v5.com1.z0.glb.clouddn.com/89494.png" alt="">

>可以看到它的headers

<img src="http://7xp9v5.com1.z0.glb.clouddn.com/89494.png" alt="">

>所以我们就可以通过python3提供的模块来获得它的response，再通过正则来提取出我们想要的数据

##　**代码的实现**

**360搜索首页**

```
#coding:UTF-8
import urllib.request
import urllib
import re
import time
from random import choice

iplist=['110.73.142.124','119.188.94.145','119.48.180.182']  #用IP代理，从网上复制下来的几个免费的

list=["编程","前端"]
for item in list:
    ip=choice(iplist)
    gjc=urllib.parse.quote(item)
    url="https://sug.so.360.cn/suggest?callback=suggest_so&encodein=utf-8&encodeout=utf-8&format=json&fields=word,obdata&word="+gjc
    headers={
             "Get":url,
             "Host":"sug.so.360.cn",
             "Referer":"https://www.so.com/?src=so.com",
             "User-Agent":"Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/44.0.2403.157 UBrowser/5.5.10106.5 Safari/537.36"
             }
    proxy_support=urllib.request.ProxyHandler({'http':'http://'+ip})  #以下三行为使用IP代理
    opener=urllib.request.build_opener(proxy_support)
    urllib.request.install_opener(opener)
    
    req=urllib.request.Request(url)
    for key in headers:
        req.add_header(key, headers[key])
    
    html=urllib.request.urlopen(req).read().decode()
    ss=re.findall("\"word\":\"(.*?)\"",html)
    for item in ss:
        print(item)
```


**百度搜索首页**

```
# coding:UTF-8
import urllib.request
import urllib
import re
import time
from random import choice

iplist=['110.73.142.124','119.188.94.145','119.48.180.182']
list=["编程","前端"]


for item in list:
    ip=choice(iplist)
    gjc=urllib.parse.quote(item)
    # gjc="集团"
    # gjc=urllib.parse.quote(gjc)
    # print(gjc)
    url="https://sp0.baidu.com/5a1Fazu8AA54nxGko9WTAnF6hhy/su?wd="+gjc+"&json=1&p=3&sid=18285_1460_18283_12826_18560_17001_17073_15526_11784_18081_18017&csor=2&cb=jQuery110205117536815814674_1454926196994&_=1454926197002"
    headers={
             "Get":url,
             "Host":"sp0.baidu.com",
             "Referer":"https://www.baidu.com/",
             "User-Agent":"Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/44.0.2403.157 UBrowser/5.5.10106.5 Safari/537.36"
             }
    proxy_support=urllib.request.ProxyHandler({'http':'http://'+ip})
    opener=urllib.request.build_opener(proxy_support)
    urllib.request.install_opener(opener)
    
    req=urllib.request.Request(url)
    for key in headers:
        req.add_header(key, headers[key])
    
    html=urllib.request.urlopen(req).read().decode('gb2312')
    ss=re.findall("\"s\":\[(.*?)\]",html)
    for item in ss:
        pass
    q=re.findall("\"(.*?)\"",item)
    for item in q:
       print(item)
    # time.sleep(6)
```

##　**代码运行结果**

**发现360搜索和百度搜索的关键词联想一样**

<img src="http://7xp9v5.com1.z0.glb.clouddn.com/789465113.png" alt="">

---

>可自由转载、引用，但需署名作者且注明文章出处。






