title: css编写规范
date: 2015-12-12
categories:
  - 前端
  - css
tags:
  - css
---
关于css编写书写顺序和规范
学习前端几个月了，都还没有好好地看看css编写规范，觉得自己代码写得都很乱，现在稍微从网上找了些来总结一下，当然并不完整，选一些比较重要的写写我也记一下

---

## **CSS书写顺序**

* 位置属性(position, top, right, z-index, display, float等)
* 大小(width, height, padding, margin)
* 文字系列(font, line-height, letter-spacing, color- text-align等)
* 背景(background, border等)
* 其他(animation, transition等)

>总体来说就是先写定位，再写盒模型，再写其他的
通过规范的css书写顺序可以提升浏览器渲染dom的速度
<!--more-->

## **CSS书写规范**

* tab四个空格缩进（这个我还被吐槽过代码混乱哈哈，这个在sublime里可以设置哈哈）
* 尽量不要用驼峰式命名class，也不要用下划线，用-这个比较好点，也不要用大写字母
网上的解释是下划线命名的选择器不支持ie6，还有JS命名变量用_下划线，才可以加以区分
* 关于空格的使用
    - 选择器与{之间有空格
    - 属性名后要有空格
* 关于选择器
    - 每个选择器占一行
    - 不要随意使用id选择器，id的优先级高于class
* 关于样式
    - 链接的样式a:link->a:visited->a:hover->a:active
    - 去掉小数点之前的0
    - 统一0后面就不要加单位啦
* 【转】常用的CSS命名规则
头：header
内容：content/container
尾：footer
导航：nav
侧栏：sidebar
栏目：column
页面外围控制整体佈局宽度：wrapper
左右中：left right center
登录条：loginbar
标志：logo
广告：banner
页面主体：main
热点：hot
新闻：news
下载：download
子导航：subnav
菜单：menu
子菜单：submenu
搜索：search
友情链接：friendlink
页脚：footer
版权：copyright
滚动：scroll
内容：content
标签：tags
文章列表：list
提示信息：msg
小技巧：tips
栏目标题：title
加入：joinus
指南：guide
服务：service
注册：regsiter
状态：status
投票：vote
合作伙伴：partner
* 【转】id的命名
    - 页面结构
    容器: container
    页头：header
    内容：content/container
    页面主体：main
    页尾：footer
    导航：nav
    侧栏：sidebar
    栏目：column
    页面外围控制整体佈局宽度：wrapper
    左右中：left right center
    - 导航
    导航：nav
    主导航：mainnav
    子导航：subnav
    顶导航：topnav
    边导航：sidebar
    左导航：leftsidebar
    右导航：rightsidebar
    菜单：menu
    子菜单：submenu
    标题: title
    摘要: summary
    - 功能
    标志：logo
    广告：banner
    登陆：login
    登录条：loginbar
    注册：register
    搜索：search
    功能区：shop
    标题：title
    加入：joinus
    状态：status
    按钮：btn
    滚动：scroll
    标籤页：tab
    文章列表：list
    提示信息：msg
    当前的: current
    小技巧：tips
    图标: icon
    注释：note
    指南：guild
    服务：service
    热点：hot
    新闻：news
    下载：download
    投票：vote
    合作伙伴：partner
    友情链接：link
    版权：copyright
* 【转】注释的写法

```
/* Header */
内容区
/* End Header */
```

* CSS样式表文件命名

主要的 master.css
模块 module.css
基本共用 base.css
布局、版面 layout.css
主题 themes.css
专栏 columns.css
文字 font.css
表单 forms.css
补丁 mend.css
打印 print.css

>参考链接：http://www.shejidaren.com/css-written-specifications.html
