title: 学习vuejs官网示例
date: 2016-07-17
categories:
  - 前端
  - JavaScript
tags:
  - JavaScript
  - vue.js
---
<img src="https://vuejs.org.cn/images/logo.png" alt="">
讲述vuejs官方示例中学习到的东西
<!--more-->

## **Markdown 编辑器 Example**
> **极简的 Markdown 编辑器。**

> 1. 需要引入`marked.js` 
> 2. 通过构造函数Vue创建了一个Vue的根实例
> 3. 通过`el`为实例挂载元素
> 4. 这个例子在`textarea`表单绑定`v-model`
> 设置了`debounce="300"`,用这个包装处理器让触发更新延迟300ms，来有效地减少重复的无用的提交
> 5. `data`为实例中的数据对象
> 6. 本例子中`v-html`，通过`textarea`的`v-model`的改变，通过`v-html`更新`div`中的`innerHTML`，实现数据更新
> 7. Vue实例中通过`filters`为`marked: marked`,在html中`<div v-html="input | marked"></div>`实现markdown的转换
> 8. 当你设置一个元素为 `box-sizing: border-box;` 时，此元素的内边距和边框不再会增加它的宽度

## **GitHub 提交 Example**
> **这个例子通过 GitHub 的 API 获取 Vue.js 最近的提交记录，并展示为一个列表。你可以在 master 和 dev 分支之间切换。**
> 1. `v-bind:href`可缩写为`:href`
> 2. `watch`,键是观察表达式，值是对应回调
> 3. `methods`为实例方法
> 4. 这个例子整体思路便是一个选择表单为绑定`v-model`，通过它来选择master和dev，再通过实例方法`methods`里的方法`fetchData`原生的`xhr`去获取数据，然后通过`v-for`循环渲染到页面

## **Firebase + 验证 Example**
> **这个例子使用 Firebase 作为后台数据存储，并与客户端实时同步 (你可以在多个浏览器标签中打开它)。另外，它使用计算属性实时校验，并且在添加/删除项目时触发 CSS 过渡效果。**
> 1. `computed`为实例计算属性
> 2. 为了应用过渡效果，需要在目标元素上使用 `transition` 特性,具体查看官网[过渡](http://vuejs.org.cn/guide/transitions.html#CSS-过渡)
> 3. 这个例子通过firebase后台，调用API存储，通过`computed`验证合法性，通过css配合`transition`实现删除和添加user时的过渡效果。

## **表格组件 Example**
> **这个例子创建了一个可复用的表格组件，用外部数据使用它。**
> 1. 注册组件，传入一个选项对象（自动调用 Vue.extend）
   `Vue.component('my-component', { /* ... */ })`
> 2. 组件实例的作用域是孤立的。这意味着不能并且不应该在子组件的模板内直接引用父组件的数据。可以使用`props` 把数据传给子组件。
> 3. `v-bind:class`缩写为`:class`
> 4. 此例子通过注册语法糖传入选项对象，创建组件，通过`props`传递数据，通过`filterBy`与表单绑定过滤数据，用`orderBy`来排序

## **树状视图 Example**
> **这个例子实现了一个简单的树状视图，演示如何递归使用组件。**
> 1. 此例子用递归组件，组件在模板楼内递归地调用自己
> 2. `Vue.set( object, key, value )`设置对象的属性,使之响应


---

>可自由转载、引用，但需署名作者且注明文章出处。