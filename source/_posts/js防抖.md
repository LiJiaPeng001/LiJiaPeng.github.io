---
title: 关于js防抖和节流
date: 2018-12-28 16:49:47
tags: JS
---

### JS 防抖

&emsp;&emsp;最近在做公司商城项目时候，有个需求就是购物车里边，数量 input 框需要根据你当前输入的值，每次输入都要发送更新请求，然后后端进行计算，前端重新渲染页面，那么问题来，我每次输入一个数字都会发送一个请求到后端，这样就非常的浪费资源，页面加载也会变慢，于是我查看了下淘宝的购物车页面，发现他们是输入框输入数字停顿大概零点几秒后再开始发送请求，而不是像我这样每次输入都发送，然后我就找到了 js 防抖

原文网址：https://juejin.im/post/5b8de829f265da43623c4261?utm_source=gold_browser_extension
我把原文缩减了下写了个小 demo
直接上代码

```
html：
  <input id='debounce' />
JS:
var timer = false;
function debounce(delay) {
      clearTimeout(timer)  // 清除未执行的代码，重置回初始化状态
      timer = setTimeout(function () {
            console.log('函数鸡柳')
      }, delay)
}
let inputb = document.getElementById('debounce')
inputb.addEventListener('keyup', function (e) {
   debounce(500)
})
```

函数防抖的要点，需要一个 setTimeout 来辅助实现,延迟执行需要跑的代码
如果方法多次触发，则把上次记录的延迟执行代码用 clearTimeout 清掉，重新开始
多用于用户名邮箱等等认证，或购物车数量

### JS 节流

规定在一个单位时间内，只能触发一次函数。如果这个单位时间内触发多次函数，只有一次生效

```
<input type="text" id="d1">

var time = 0
document.getElementById("d1").onkeyup=function(){
    set()
}
function set(){
    var date = new Date().getTime()
    if(date-time>=2000){
        timer = setTimeout(e=>{
            console.log("红烧")
            time = date
        },2000)
    }
}
```

然后你就会发现他会每隔 2 秒执行一次
节流我的思路就是每次输入的时候获取当前的毫秒数，如果当前毫秒数减去上一次的毫秒数>=2000 了，这时候开始进行定时器的逻辑，定时器结束后把结束的时间换成当前的 time
函数节流应用的实际场景，多数在监听页面元素滚动事件的时候会用到。因为滚动事件，是一个高频触发的事件

### 总结

- 函数防抖和函数节流都是防止某一时间频繁触发，但是这两兄弟之间的原理却不一样
- 函数防抖是某一段时间内只执行一次，而函数节流是间隔时间执行
