title: 理解Promise简单实现的背后原理
date: 2017-03-23
categories:
  - 前端
  - javascript
  - promise
tags:
  - promise
---

在写javascript时我们往往离不开异步操作，过去我们往往通过回调函数多层嵌套来解决后一个异步操作依赖前一个异步操作，然后为了解决回调地域的痛点，出现了一些解决方案比如事件订阅／发布的、事件监听的方式，再后来出现了Promise、Generator、async/await等的异步解决方案。co模块使用了Promise自动执行Generator，async/await这个Node7.6开始默认支持的最新解决方案也是依赖于Promise,所以了解Promise是非常有必要的，而理解它背后的实现原理则能在使用它的时候更加游刃有余。
<!--more-->
## 实现一个简单的异步方案
我们知道`Promise`实现多个相互依赖异步操作的执行是通过`.then`来实现的，我们会不由发出疑问，后面的操作是如何得知前面异步操作的完成的，我们可能会产生一种想法，后面有一个函数在一直监听着前面异步操作的完成，你说的是发布／订阅模式？Promise的实现个人觉得也有点发布／订阅的味道，不过它因为有`.then`的链式调用，又没有使用on/emit这种很明显的订阅／发布的东西，让实现变得看起来有点复杂

不过我们可以先想想发布／订阅是怎么做的，首先有一个事件数组来收集事件，然后订阅通过on将事件放入数组，emit触发数组相应事件，嗯嗯，这并不是很复杂，理解了这个以后，我们开始真正地讲解实现。

Promise其实内部也有一个`defers`队列存放事件，`.then`的事件就在里面,聪明的你就想到了，程序开始执行的时候，`.then`就已经放入下一个事件，然后后面当异步操作完成时，`resolve`触发事件队列中的事件，便完成了一个`.then`操作， 其实到这里我们就可以很快地想出一种解决方案，每次异步操作完成通过`resolve`触发事件并将事件从事件队列中移除，通过事件队列中的事件的`resolve`使事件的触发持续下去，我们可以用十几行代码就可以实现这样的逻辑，实现一个简单的异步编程方案

``` javascript
function P(fn) {
    var value = null;
    var events = [];
    this.then = function(f) {
        events.push(f);
        return this;
    }
    function resolve(newValue) {
        var f = events.shift();
        f(newValue, resolve);
    }
    fn(resolve);
}

function a() {
    return new P(function(resolve) {
        console.log("get...");
        setTimeout(function() {
            console.log("get 1");
            resolve(1);
        }, 1000)
    });
}
a().then(function(value, resolve) {
    console.log("get...");
    setTimeout(function() {
        console.log("get 2");
        resolve(2);
    }, 1000)
}).then(function(value, resolve) {
    console.log(value)
})
```

这样就得到控制台如下的结果

```
get... 
get 1
get...
get 2
2
```
我们当然只是初步地简单接触异步的一种方案，我们没有`reject`,没有进行错误处理，这不是完整的，读者想要扩展的话，可以再自行去实现,接下来我们要去接触真正的 Promises/A+规范所实现的Promise
## 简单理解Promise/A+规范的promise背后的实现
**Promise/A+规范：** https://promisesaplus.com/

我是通过这篇[《剖析 Promise 之基础篇》](http://tech.meituan.com/promise-insight.html)学习的，本文后面使用的代码也是来自于此文，读者可以先看完上文再来加深理解。

假设我们有一个场景，我们需要异步先获取到用户id,再通过用户id异步再获取到用户名字，拿到名字输出，
我们很迅速地写出Promise的代码（因为不是Promise的完整实现，就用MyPromise)

``` javascript
function getID() {
  return new MyPromise(function(resolve, reject) {
    console.log("get id...");
    setTimeout(function() {
      resolve("666");
    }, 1000);
  })
}
function getNameByID(id) {
  return new MyPromise(function(resolve, reject) {
    console.log(id);
    console.log("get name...");
    setTimeout(function() {
      resolve("hjm");
    }, 1000);
  })
}
getID().then(getNameByID).then(function(name) {
  console.log(name);
}, function(err) {
  console.log(err);
});
```

正确输出了我们想要的结果，后面的fn拿到了前面resolve的value

```
get id...
666
get name...
hjm
```

其实我们最大的疑问会在于两个promise它是如何通过`.then`连接起来的,一图胜千言。

<img src="http://7xp9v5.com1.z0.glb.clouddn.com/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202017-03-23%20%E4%B8%8B%E5%8D%8812.54.09.png" alt="">

> 橙色：是刚开始初始化产生的东西（一堆.then产生的）

> 紫色：是异步开始执行后的一系列流程

第一眼看起来很复杂，下面我们慢慢去一步步拆开

### 先抛开紫色的不看

<img src="http://7xp9v5.com1.z0.glb.clouddn.com/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202017-03-23%20%E4%B8%8B%E5%8D%884.41.53.png" alt="">

每个Promise实例包含状态`state`、事件队列`defers`、`value`、`resolve`、`reject`

还有一个`handle`函数，当状态为`pending`时是将Defered{}（包含onFulfilled、onRejected、resolve、reject）放入队列的操作，当状态为`fulfilled`或`rejected`会执行相应事件的函数`onFulfilled`/`onRejected`并且resolve返回的东西

然后为了实现串行Promise，`.then`其实又产生了一个新的Promise实例作为中间Promise,
它将`then`里的函数再与自己的实例中的`resolve`,`reject`共同组成一个Defered{}（包含onFulfilled、onRejected、resolve、reject）,注意这里非常关键，它放入了自己实例的`resolve`、`reject`，这将是串行Promise桥梁的关键之处（通过闭包实现的），用`handle`函数把这个对象放入前一个Promise实例的事件队列里



### 异步开始！
<img src="http://7xp9v5.com1.z0.glb.clouddn.com/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202017-03-23%20%E4%B8%8B%E5%8D%8812.54.09.png" alt="">

> 紫色：是异步开始执行后的一系列流程

> 跟着标号看～假如前面的东西理解的话，你会看得下去的～哈哈

1. getID setTimeout 1000s时间到,调用实例的resolve(
"666")
2. 当前Promise实例的状态改变（等待=>完成），实例的value(=>666)
3. 调用当前handle函数，由于状态是fulfilled,传入当前value 666进入事件队列中的相应函数（它返回的也是一个Promise），getNameByID开始执行
4. 调用resolve通过判断返回的是不是Promise,如果是的话就调用当前返回的.then
5. 调用.then将前面实例的resolve、reject传过去作为onFulfilled、onRejected
6. 可以仔细看图的这条线，这样就很奇妙地将这个事件队列中返回的promise和下一个.then中间Promise串起来了，它们引用都是同样的resolve、reject
7. 当第二个异步操作getNameByID setTimeout 1000s再次执行完成,调用实例的resolve("hjm")
8. 当前Promise实例的状态改变（等待=>完成），实例的value(=>hjm)
9. 调用当前handle函数，由于状态是fulfilled,传入当前value hjm进入事件队列中的相应函数，其实就是下一个中间Promise的resolve("hjm")
10. 当前中间Promise实例的状态改变（等待=>完成），实例的value(=>hjm)
11. 调用当前handle函数，由于状态是fulfilled,传入当前value hjm进入事件队列中的相应函数，打印出console.log("hjm"),成功拿到name
12. 调用resolve,发现事件队列已经没有东西了，程序也就结束了

此文的代码地址在github上：https://github.com/BUPT-HJM/study-js/blob/master/%E5%85%B6%E4%BB%96/promise.js

想要自己运行的同学可以试试看，理清了整个流程会对Promise清晰很多～

## Promise的小test
这两个问题是从[饿了么 node-interview](https://github.com/ElemeFE/node-interview/blob/master/sections/event-async.md#promise)摘出
### 判断输出以及相应的时间

``` javascript
let doSth = new Promise((resolve, reject) => {
  console.log('hello');
  resolve();
});

setTimeout(() => {
  doSth.then(() => {
    console.log('over');
  })
}, 10000);
```
### 判断输出顺序

``` javascript
setTimeout(function() {
  console.log(1)
}, 0);
new Promise(function executor(resolve) {
  console.log(2);
  for( var i=0 ; i<10000 ; i++ ) {
    i == 9999 && resolve();
  }
  console.log(3);
}).then(function() {
  console.log(4);
});
console.log(5);
```

### 解答
其实这两题用三个tip就可以解决
- Promise函数调用就执行
- [Promise/A+规范](https://promisesaplus.com/)中then置于当前事件循环的末尾
- setTimeout(fn,0)会在下一个事件循环出现

> 这里往深处分析，涉及到event loop、macro-task、micro-task等一些东西，个人也没怎么深入了解，就不加以深入分析了
> 
> 有兴趣的同学可以阅读： https://github.com/creeperyang/blog/issues/21

回到题目，第一题由tip1,所以是马上console.log(hello),然后隔10s后输出over

第二题用用三个tip，Promise执行输出2，调用resolve，再输出3，然后调用then将输出4置于事件循环末尾，然后输出5，到达末尾，输出4，下一个事件循环，输出刚开始的1，所以顺序是23541

### **最后**

谢谢阅读~
欢迎follow我哈哈[https://github.com/BUPT-HJM](https://github.com/BUPT-HJM)
欢迎继续观光我的博客~

**欢迎关注**
<img src="http://7xp9v5.com1.z0.glb.clouddn.com/qrcode_for_gh_f4166e610ee5_344.jpg" alt="">
---

>可自由转载、引用，但需署名作者且注明文章出处。