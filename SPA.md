## SPA
---
### 历史问题
从前，前端开发都是后端负责业务逻辑、数据处理和渲染页面模板并返回给前端，而前端仅仅是个展示页面的作用。在这种模式下，用户每执行点击链接、提交表单等动作时，浏览器总得重新发起新请求、得到新页面并渲染，这带来的问题就是等候新页面时间太久、体验不好。
### 现在
现在产生了SPA的概念，SPA是single-page application的缩写，就是单页面应用，它可以动态地重写当前的页面来与用户交互，而不需要频繁地重新加载整个页面。SPA应用的流畅性让Web应用更像桌面端或Native应用。此外，相对于传统的前后端不分离的网页开发，SPA做到了前后端分离————即把页面逻辑和页面渲染的任务留给前端完成，后端只负责业务逻辑和数据处理，并把处理后的数据返回给前端使用。目前Vue、React、Angular三大框架都采用了SPA原则。单页面应用意味着前端掌握了主动权，也让前端代码更复杂和庞大，因而凸显了前端**模块化**、**组件化**和**架构设计**的重要性。
### 原理
SPA的一个重要实现是当路由改变时，页面不会重新加载（刷新），而是动态加载变化的部分（DOM树动态改变它需要改变的部分，整个HTML的大框架是基本不变的）。要达到路由（url）改变而页面不刷新的目的，需要借助以下两种手段：
1. 利用H5在window.history对象中新增的两个方法：**pushState** 和 **replaceState**；
2. 利用location.hash；
#### 先复习一下history对象：
- history.back(): 与在浏览器点击后退按钮效果相同；
- history.forward(): 与在浏览器点击前进效果相同；
- history.go(): 接受一个整数作为参数，移动到该整数指定的页面，如go(1)相当于forward()，go(-1) 相当于back()，go(0)相当于刷新当前页面；
#### 有学习一下新知识：
```javascript
window.history.pushState(state, title, url);
```
- state 是状态对象，通过history.state可以读取；
- title 表示新页面的标题，但是现在的所有浏览器都会忽略这个字段，所以可以传 null；
- url 是新页面地址，必须是和当前页面在同一个域；

# 未完待续，睡觉去
