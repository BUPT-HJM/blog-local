title: ajax+jsonp跨域搜索的实现
date: 2016-01-10
categories:
  - 前端
  - JavaScript
  - ajax
tags:
  - ajax
---
## **静态内容的实现**

**有一个居中背景图，图里嵌套一个logo加表单form**

```
<body>
  <div class="bg-div">
    <div class="search-box">
      <div class ="logo"></div>
      <form id="search-form" action="https://cn.bing.com/search" target="_blank" id="search-form">
        <input type="text" class="search-input-text" id="search-input" autocomplete="off" name="q">
        <input type="submit" class="search-input-button" value="" id="search-submit">
      </form>
    </div>
  </div>
  <div class="suggest" id="search-suggest" style="display:none">
    <ul id="search-result">
      <li>搜索结果1</li>
      <li>搜索结果2</li>
    </ul>
  </div>
</body>
```
<!--more-->
**样式实现**

```
  body {
      background-color: #333;
    }
    .bg-div {
      background-image: url(river.jpg);
      width: 1228px;
      height: 690px;
      margin: 0 auto;
      position: relative;
    }
    .logo {
      margin: -4px 18px 0 0;
      background-image: url(logo.png);
      width: 107px;
      height: 53px;
      float: left;
    }
    form {
      float: left;
      background-color: #fff;
      padding: 5px;
    }
    .search-input-text {
      border: 0;
      float: left;
      height: 25px;
      line-height: 25px;
      outline: none;
      width: 350px;
    }
    .search-input-button {
      border: 0;
      background-image: url(search-button.png);
      width: 29px;
      height: 29px;
      float: left;
      cursor: pointer;
    }
    .search-box {
      position: absolute;
      top: 200px;
      left: 300px;
    }
    .suggest {
      width: 388px;
      background-color: #fff;
      border: 1px solid #999;
    }
    .suggest ul {
      list-style: none;
      margin: 0;
      padding: 0;
    }
    .suggest ul li {
      padding: 3px;
      font-size: 14px;
      line-height: 25px;
      cursor: pointer;
    }
    .suggest ul li:hover {
      text-decoration: underline;
      background-color: #e5e5e5;
    }
```

**实现的效果**

<img src="http://7xp9v5.com1.z0.glb.clouddn.com/%E5%9B%BE%E5%83%8F%201.png" alt="">

## **js部分的实现**

**引入jq**

```
<script src="jquery.min.js"></script>
```

**完成jsonp+ajax跨域访问**
```
$(document).ready(function() {
        $("#search-input").bind('keyup', function () {
            var inputText = $("#search-input").val();
            var callback= function (data) {
                var d = data.AS.Results[0].Suggests;
                var html = "";
                for (var i = 0; i < d.length; i++) {
                    html += '<li>' + d[i].Txt + '</li>';
                }
                $("#search-result").html(html);
                $("#search-suggest").css({
                    top: $('#search-form').offset().top + $("#search-form").height() + 10,
                    left: $('#search-form').offset().left,
                    position:'absolute'
                }).show();
            };
            $.ajax({
                type: "get",
                async: false,
                url: "http://api.bing.com/qsonhs.aspx?type=cb&cb=callback&q=" + inputText,
                dataType: "jsonp",
                jsonp: "callback",
                jsonpCallback:"callback",
                success: function (data) {
                    callback(data);
                },
                error: function (data) {
                    console.log(data);
                }
            });
        });
        $(document).bind('click',function(){
           $('#search-suggest').hide();
        });
        $('#search-suggest').delegate('li','click', function () {
            var keyword=$(this).text();
           location.href='http://cn.bing.com/search?q='+keyword;
        });
    });
```

**代码详解**

**1.实现智能提示框的定位**

用jq的css()方法控制`search-form`的position实现定位

**2.完成ajax部分**

通过jq的ajax()方法加载远程数据，通过获取的远程数据来填充智能提示框，通过datatype为jsonp实现跨域访问，避开同源策略

```
     $.ajax({
                type: "get",//请求方式
                async: false,//默认为true，默认情况为异步请求，同步请求改为false，我们这里暂且改为flase，让用户在请求完成后再进行下一步操作
                url: "http://api.bing.com/qsonhs.aspx?type=cb&cb=callback&q=" + inputText,//发送请求的地址
                dataType: "jsonp",//预期浏览器返回的数据类型
                jsonp: "callback",//在一个 jsonp 请求中重写回调函数的名字
                jsonpCallback:"callback",//为 jsonp 请求指定一个回调函数名
                success: function (data) {//请求成功后的回调函数
                    callback(data);
                },
                error: function (data) {//请求失败调用的函数
                    console.log(data);
                }
            });
```


**3.完成智能提示的填充**

总的还是要通过绑定用户的可以keyup来触发函数

```
  var inputText = $("#search-input").val();
            var callback= function (data) {
                var d = data.AS.Results[0].Suggests;
                var html = "";
                for (var i = 0; i < d.length; i++) {
                    html += '<li>' + d[i].Txt + '</li>';
                }
                $("#search-result").html(html);
```

**上述的`data.AS.Results[0].Suggests` 是什么？**

用F12打开调试进入Network->Preview可以看到

<img src="http://7xp9v5.com1.z0.glb.clouddn.com/%E5%9B%BE%E5%83%8F%202.png" alt="">

返回的数据是

```
if(typeof callback == 'function') callback({"AS":{"Query":"你好","FullResults":1,"Results":[{"Type":"AS","Suggests":[{"Txt":"你好小娜","Type":"AS","Sk":"","HCS":0.577300},{"Txt":"你好啊","Type":"AS","Sk":"AS1"},{"Txt":"你好外星人","Type":"AS","Sk":"AS2"},{"Txt":"你好陌生人","Type":"AS","Sk":"AS3"},{"Txt":"你好的日语","Type":"AS","Sk":"AS4"},{"Txt":"你好你好","Type":"AS","Sk":"AS5"},{"Txt":"你好菜鸟","Type":"AS","Sk":"AS6"},{"Txt":"你好 旧时光","Type":"AS","Sk":"AS7"}]}]}} /* pageview_candidate */);
```


接着通过` data.AS.Results[0].Suggests`我们就可以获得我们所需的数据，填充入下面的提示框，完成填充

## **最终完成的效果**

<img src="http://7xp9v5.com1.z0.glb.clouddn.com/%E5%9B%BE%E5%83%8F%203.png" alt="">

>参考链接：
http://www.w3school.com.cn/jquery/ajax_ajax.asp
http://www.imooc.com/learn/21

---

>可自由转载、引用，但需署名作者且注明文章出处。









