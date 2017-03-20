title: 基于node的stream pipe实现反向代理
date: 2017-03-20
categories:
  - 前端
  - nodejs
  - stream
tags:
  - nodejs
  - 反向代理
---

## **反向代理**
讲到反向代理，就先提下正向代理与反向代理的区别，正反是相对代理对象而言的，正向则是代理对象是客户端，反向的代理对象是服务端，代理的概念就是中介者，正向代理比如一些vpn,它作为客户端的代理对象，客户端的请求都让它来代替请求，服务端只知道那个代理对象，不知道你的请求，然而反向代理则不同，比如著名的反向代理服务器`nginx`，就起到这样一个功能，很多大型网站都不止一台服务器，然而我们就只会请求它一个地址，作为反向代理服务器，它帮我们把请求转发到相应的服务器并将返回的内容给我们，但是你并不知道那些真实返回请求的服务器是什么。
通俗来讲，对代理对象的正反而言，其实就是正向代理充当客户端，反向代理充当服务端
<!--more-->

> 我们实现的反向代理镜像站点，还需要实现内容的替换

## **使用nginx进行反向代理**
nginx作为著名的反向代理服务器，必须谈一下它～
我们要实现的本机访问test.com就是访问baidu.com，并且替换`百度`为`test`
1. 安装nginx-full并且安装相应替代插件
`brew install nginx-full --with-subs-filter-module --with-sub`

2. 配置nginx.conf
需要关闭gzip，subs_filter才能正常工作
```
server {
    listen  80;
    server_name  *.test.com test.com;
    location / {
        proxy_pass https://www.baidu.com;
        proxy_set_header Accept-Encoding '';
        subs_filter '百度' '头条';
       }
    }
```
3. 配置etc/hosts
`127.0.0.1 test.com www.test.com`

4. 开启nginx访问test.com
`sudo nginx`

## **直接使用stream pipe实现代理**

### **stream与pipe**
`stream`即流，可以理解为运载数据的流，`HTTP request`和`process.stdout`这些都是流的实例，分为可写流、可读流、可读可写流，使用`stream`可以实现数据的流式处理，`pipe`是`stream`的强大之处，跟unix中的`|`类似，可以将一个流的输出灌入另一个流的输入，很多强大的工具都使用stream pipe去实现流式处理，比如`gulp`、`Browserify`等等，本文的重点不在于介绍node的stream模块，而且网上也有文章深入地介绍了stream的原理和应用，给出几个比较好的链接，供读者阅读学习

- [stream-handbook](https://github.com/substack/stream-handbook)
- [stream-handbook中文版](https://github.com/jabez128/stream-handbook)
- [streamify-your-node-program](https://github.com/zoubin/streamify-your-node-program)
- [node中的Stream－Readable和Writeable解读](http://www.cnblogs.com/accordion/p/5560531.html)
- [深入node之Transform](http://web.jobbole.com/88100/)

还有几张助于理解的图
（来自[美团点评团队](http://tech.meituan.com/stream-internals.html))
<img src="http://tech.meituan.com/img/stream-how-data-comes-out.png" alt="">
(来自[royalrover](http://www.cnblogs.com/accordion/p/5560531.html))
<img src="http://7xp9v5.com1.z0.glb.clouddn.com/o_ttt.jpg" alt="">

### **如何用pipe实现反向代理**
其实原理很简单
- 起一个`express`服务器作为反向代理服务器，作为服务端
- 当我们对服务端访问时，request进入服务端，request是一个可读流，我们用`pipe`把它接到真实的服务器上，怎么接呢，我们可以通过http request,图方便我使用了node的模块`request`，`req.pipe(r)`,这样就请求到了真实的服务端、
- 那返回的内容呢，在这里我们可以看到`pipe`的强大之处，pipe后返回目标流的引用，对，我们可以继续`pipe`,`req.pipe(r).pipe(res)`,这样其实就实现了反向代理
- 当然这个只能实现简单的get请求，对于post或者put请求带参数的，我们还需在`request`模块中将参数传递
- 如果还有鉴权部分，我们需要在`headers`中添加cookie
- 最后其实要实现镜像站点，很多情况下我们还是需要替换部分内容，所以我们需要在pipe的过程中拿到返回的`MIME`,判断内容类型，而且需要注意的是stream传来的是一片一片的buffer而不是完整的内容，当然你可以对每一块buffer进行处理，但是通常通过正则方式或者使用cheerio来处理，我们往往需要整块内容，可以监听data拼接buffer,或者使用`concat-stream`直接拿到拼接成一块的buffer,再`Buffer.toString()`做处理，如果有压缩`gzip`的话还需要`zlib`去解压一下再去处理

### **stream pipe实例代码**
一个简单的反向代理服务器，可以修改为配置为其他地址，注意事项就是上面所说的

``` javascript
function reverseController(req, res) {
    var cookie = APP_CONF.cookie;
    // 反向代理链接
    var options = {
        url: APP_CONF.reverse_url + req.originalUrl,
        headers: {
            //cookie: APP_CONF.cookie
        }
    }
    var url = APP_CONF.reverse_url + req.originalUrl;
    var r = null;
    if (/POST|PUT/i.test(req.method)) { //假如是post／put请求
        r = request.post(Object.assign(options, { json: req.body }))
            .on('response', function(response) {
                // 响应相应post/put请求
            });
        req.pipe(r).pipe(res);
    } else {
        var mime = '';
        var isGzip = false;
        r = request(options)
            .on('response', function(response) {
                mime = response.headers['content-type'];
                if (response.headers['content-encoding'] == 'gzip') {
                    isGzip = true;
                }
                res.writeHead(response.statusCode, response.headers);
            });
        // req 可读流 ==传递chunk==> r 可写流 ==返回目标流传递chunk==> concat 可写流
        req.pipe(r).pipe(concat(function(body) {
            // 拦截内容，根据mime去处理不同body
            if (mime && mime.indexOf("text/html;") !== -1) {
                if (isGzip) {
                    zlib.gunzip(body, function(err, decoded) {
                        var $ = cheerio.load(decoded.toString());
                        // html相关处理
                        // cheerio
                        body = $.html();
                    });
                } else {
                    var $ = cheerio.load(decoded.toString());
                    // html相关处理
                    // cheerio
                    body = $.html();
                }
            }
            res.end(body);
        }));
    }
}
```

完整代码github地址:[https://github.com/BUPT-HJM/study-js/blob/master/study-nodejs/study-stream/reverse-proxy-pipe.js](https://github.com/BUPT-HJM/study-js/blob/master/study-nodejs/study-stream/reverse-proxy-pipe.js)

### **其他方式**
可以通过网上一些现成的中间件、模块来实现，不过大体原理也是通过pipe来实现

**使用node-http-proxy反向代理**

> 使用middleware中的harmon进行内容修改

- harmon: https://github.com/No9/harmon
- node-http-proxy: https://github.com/nodejitsu/node-http-proxy

**使用hoxy进行代理**

> 自带cheerio，修改方便,并且hoxy代码拦截流程以及替换流程比较直接，这个其中的拦截很方便使用，可惜它跟express不好结合

- hoxy: https://github.com/greim/hoxy
- 文档：http://greim.github.io/hoxy/#main-module

### **最后**

谢谢阅读~
欢迎follow我哈哈[https://github.com/BUPT-HJM](https://github.com/BUPT-HJM)
欢迎继续观光我的博客~

**欢迎关注**
<img src="http://7xp9v5.com1.z0.glb.clouddn.com/qrcode_for_gh_f4166e610ee5_344.jpg" alt="">
---

>可自由转载、引用，但需署名作者且注明文章出处。