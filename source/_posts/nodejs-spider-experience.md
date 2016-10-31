title: 个人nodejs爬虫相关模块小整合
date: 2016-10-31
categories:
  - 前端
  - nodejs
  - 爬虫
tags:
  - nodejs
  - 爬虫
---

用node爬虫也有一段时间了，最近也帮好友爬了XX网的10万+的数据，这个月还没有写博客，就来写写自己爬虫的经验以及自己常用的一些模块/库吧~
<!--more-->

爬虫关键步骤都围绕在于`请求`、 `获取数据`、`处理数据`，当然还有应对一些反爬虫的策略，比如伪造headers，ip代理等等，下文就主要围绕nodejs我常用的模块和经验谈起

### **请求和获取数据模块**

**[superagent](https://github.com/visionmedia/superagent)** 

名字就叫做超级代理，是一个非常实用的http请求模块，常用于`get`、`post`等请求，对于nodejs爬虫我主要就用它来实现请求操作，还有一些表单post操作，模拟ajax请求等,z支持链式调用
``` javascript
superagent.post(url)
    .set(headers) //设置请求头
    .set('Cookie', cookie) // 设置cookie
    .type('form')
    .send({
        //表单数据
    })
    .end(function(err, res) {
        //数据与错误处理
})
```
> **中文文档：**[https://cnodejs.org/topic/5378720ed6e2d16149fa16bd](https://cnodejs.org/topic/5378720ed6e2d16149fa16bd)


**[superagent-charset](https://github.com/magicdawn/superagent-charset)**

这个是上面那个模块的拓展,因为`superagent`只支持`UTF-8`,用了这个库可以指定编码,当你用`superagent`爬出乱码时可以使用它

``` javascript
superagent.get(url)
    .set(headers) 
    .charset('gbk') //指定编码
    .end(function(err, res) {
        if (err) {
            return console.err(err);
        }
    })
```


**[superagent-retry](https://github.com/segmentio/superagent-retry)**

当你遇到以下错误的时候,可以使用它,之前我用了它确实错误问题就解决了
- ECONNRESET
- ECONNREFUSED
- ETIMEDOUT
- EADDRINFO
- ESOCKETTIMEDOUT
- superagent client timeouts
- bad gateway errors (502, 503, 504 statuses)
- Internal Server Error (500 status)

``` javascript
var superagent = require('superagent');
require('./superagent-retry')(superagent);
superagent.get(url)
    .set(headers_visit)
    .retry(2) //在响应前请求两次
    .end(function(err, res) {
        if (err) {
            return console.err(err);
        }
     })
```

### **处理数据模块**

**[cheerio](https://github.com/cheeriojs/cheerio)**

这是一个非常好用的处理模块,是jQuery的子集,可以使用大量jQuery的语法来获取你爬虫请求到的数据,用正则真是太不友好~

``` javascript
superagent.get(url)
    .set(headers)
    .end(function(err, res) {
        if (err) {
            return console.err(err);
        }
        $ = cheerio.load(res.text);
        // $(".XXX")选中class
        // $("#XXX")选中id
        // ...
        })
```

> **中文文档:** [https://cnodejs.org/topic/5203a71844e76d216a727d2e](https://cnodejs.org/topic/5203a71844e76d216a727d2e)

**fs**

它虽然是一个模块,但并不需要从npm下载(npm里的那个就一句话`console.log("I'm fs modules");`),是nodejs自带的文件系统模块,直接引入即可,提供了nodejs本地读写的能力,可以写入读取本地文件,我们爬虫如果不需要写入数据库,写入本地的话,就必须得用到它了
```
var fs = require('fs');
// 读取文件
fs.readFile('input.txt', function (err, data) {
  if (err) {
     return console.error(err);
  }
});
// 写入文件
fs.writeFile('input.txt', 'XXX',  function(err) {
   if (err) {
       return console.error(err);
   }
});
```
> **中文教程:** [http://www.runoob.com/nodejs/nodejs-fs.html](http://www.runoob.com/nodejs/nodejs-fs.html)


**excel相关**

最近爬的数据需要爬入excel,然而fs模块并不能直接写入xlsx,就得用别的途径

**[node-xlsx](https://github.com/mgcrea/node-xlsx)**

这个模块挺简单上手的,我只试过导出excel,也可以读取,觉得挺方便,具体其他可以直接看docs
```
var xlsx = require('node-xlsx');
var fs = require('fs');

var data = [
    [1, 2, 3],
    [true, false, null, 'sheetjs'],
    ['foo', 'bar', new Date('2014-02-19T14:30Z'), '0.3'],
    ['baz', null, 'qux']
];

var buffer = xlsx.build([{
    name: 'mySheet',
    data: data
}]);

fs.writeFile('test.xlsx', buffer, {
        'flag': 'w+'
    }, function(err) {
        if (err) {
            return console.error(err);
        }
        console.log("写入成功");
    });
```

> **tips:**不过因为我爬取大量数据,如果一味放数组里也会让程序崩溃,但是我开启`a+`这个fs的追加模式,发现写入有点问题,所以我最近采用了逗号分割csv的方式

**[csv](https://zh.wikipedia.org/zh-hans/%E9%80%97%E5%8F%B7%E5%88%86%E9%9A%94%E5%80%BC)**

.csv后缀文件可以以纯文本存储表格数据,然后我直接用`fs`写入,用`,`来分割列,用`\n`来分割行就解决问题了,这个简单快捷
> 当你用utf-8写入csv后,用excel直接打开是会乱码的,你可以先用记事本打开另存为ANSI编码格式,再打开就正常显示了

### **异步处理模块**

> 这个我没去调研各个模块,,就只介绍我用过的

对于异步这在node爬虫中是必须去处理的,因为fs写入文件,superagent请求这些都是异步的,我们可以用`promise`来解决,当然es6出现了`generator`生成器解决回调,还有es7的`async/await`的异步解决方案,这里就暂且不说这些了,我就只介绍下我使用的`async`模块

**[async](https://github.com/caolan/async)**

这是一个异步流程控制模块,还可以在爬虫时控制并发数目,通常来说我们爬取一堆url有规律的数据,我通常构造一个数组然后使用它来解决异步

```
// 我这里对一个有规律的url+urlArr[i]进行爬取
for (var i = 1; i <= 900000; i++) {
    urlArr.push(i);
}
// 这里是分6批执行,每一批并行,批之间按顺序
async.eachLimit(urlArr, 6, function(item, callback) {
    spider(item, callback);//爬虫函数
    // 用callback(null)代表完成
    // 用callback('err')代表错误
}, function(err) {
    if (err) {
        return console.log(err);
    }
    console.log("结束");
});
```

> 这个模块就可以解决我的异步问题了,更多用法看这位作者写的,挺详细的,[https://github.com/alsotang/async_demo](https://github.com/alsotang/async_demo)


### **其他模块**

> 这些模块就有些针对爬虫特定需求的

**[node-tesseract](https://github.com/desmondmorris/node-tesseract)**

这是验证码识别相关的

- 可以看我的文章[https://bupt-hjm.github.io/2016/05/29/buptclass/](https://bupt-hjm.github.io/2016/05/29/buptclass/)
- 还有这位前辈写的[http://think2011.net/2016/01/31/node-ocr/](http://think2011.net/2016/01/31/node-ocr/)

**[node-schedule](https://github.com/node-schedule/node-schedule)**

当我们的爬虫需要定时爬取的时候,它就可以发挥巨大作用啦

- [node.js中使用node-schedule实现定时任务实例](http://nodeclass.com/articles/78767)


**[node-ssh2sync](https://github.com/robogeek/node-ssh2sync)**

之前我爬取教务系统,因为只能校园网访问,所以我就只能本地爬取上传服务器,这个不是npm下载的模块,直接从github下载,作为我简单的上传需求,它还是挺好用的

``` javascript
var ssh2sync = require('ssh2sync');

var root_local = ... ; // Grab these from command line arguments?
var root_remote = ...;
var force = true;

ssh2sync.upload(root_local, root_remote, force, {

    //    debug: console.log,
    host: ... remote host name ...,
    port: 22,
    username: ... user name ...,
    privateKey: require('fs').readFileSync('/path/to/users/.ssh/id_dsa')
    // OR password: ... the password ..
});
```

### **最后**

谢谢阅读~
欢迎follow我哈哈[https://github.com/BUPT-HJM](https://github.com/BUPT-HJM)
欢迎继续观光我的博客~

**欢迎关注**
<img src="http://7xp9v5.com1.z0.glb.clouddn.com/qrcode_for_gh_f4166e610ee5_344.jpg" alt="">
---

>可自由转载、引用，但需署名作者且注明文章出处。