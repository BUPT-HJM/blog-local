title: nodejs学习-ejs模板引擎
date: 2016-01-05
categories:
  - 前端
  - nodejs
  - 模板引擎
tags:
  - ejs
---
# **Express使用ejs**
```
app.set('views',path.join(_dirname,'views'));
app.set('view engine','ejs');
```
<!--more-->
# **ejs标签系统**
## **<% XXX %>放js**
>例如ejs官方示例

**The Data**

```
supplies:['mop','broom','duster']
```

**The Template**
```
<ul>
    <% for(var i=0;i<supplies.length;i++) {%>
    <li><%= supplies[i] %></li>
    <% } %>
</ul>
The Result
<ul>
    <li>mop</li>
    <li>broom</li>
    <li>duster</li>
</ul>
```

## **<%= XXX %>**

>放需要被替换的html，比如可以用res.render('index',{title:'Express'})来讲页面的<%= title %>替换

## **<%- XXX %>**

>可以用include来使用公用布局，在这个模板引入其他模板

**index.ejs**

```
<%- include a %>
hi
<%- include b %>
```

**a.ejs**

```
a.ejs
```

**b.ejs**

```
b.ejs
```

最后就变成了

```
a.ejs
hi
b.ejs
```

>本文参考《nodejs》实战

---

>可自由转载、引用，但需署名作者且注明文章出处。
