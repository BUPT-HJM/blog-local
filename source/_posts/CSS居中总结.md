title: CSS居中总结
date: 2016-01-12
categories:
  - 前端
  - css
tags:
  - css
---

> css居中是布局中常见的，在看了网上一些资料后加以总结，侵删


## **水平居中**

**子元素为行内元素**
>父元素设置`text-align:center`
子元素设置为行内元素，如`display:inline-block`
(子元素内文字也会居中）

<!--more-->

**子元素为块状元素**
>子元素为块状元素，设置`margin: 0 auto`

**用CSS3+position定位**
>父元素`position: relative`
子元素`position: absolute`再加`left: 50%;transform: translateX(-50%)`
(低版本IE不兼容CSSS3新属性）

**用margin+position定位**
>子元素加一个兄弟元素
子元素`position: absolute;left: 50%;`->子元素的左边界在页面中央
兄弟元素`margin-left: -50%;width: 100%;`->移动中轴线到中央

## **div垂直居中**

**CSS3+position定位**
>父元素`position: relative`
子元素`position: absolute`再加`top: 50%;transform: translateY(-50%)`
(低版本IE不兼容CSSS3新属性）

**`tabel-cell`+`verticle-align`**
>父元素设置`display: table-cell; vertical-align: middle;`
(兼容到IE8

## **文本垂直居中**

**`table-cell`**
>div设置`display: table`
文本设置`diaplay: table-cell;verticle-align: middle`

**`line-height`+`height`**
>设置`line-height`与`height`相等
（适用于定高)

**设置绝对居中**
>同样用position完成

**`inline-block`**

```
#div{text-align:center; overflow: auto; } 
#div:before {content:'';display:inline-block;vertical-align:middle;height:100%;width:0; }
```

>仅用于单行文本

## **flex**

**水平居中**
>设置父元素`display: flex;justify-content: center;`

**垂直居中**
>设置父元素` display: flex;align-items: center;`

(兼容到IE10)

---
>可自由转载、引用，但需署名作者且注明文章出处。







