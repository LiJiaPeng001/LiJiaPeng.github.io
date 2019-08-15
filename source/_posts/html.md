---
title: html
date: 2019-07-15 17:09:46
tags: JS
---

#### 超出行隐藏

```
overflow:hidden;
text-overflow:ellipsis;
display:-webkit-box;
-webkit-box-orient:vertical;
-webkit-line-clamp:2;   参数可为任意行数
```

#### 实现一个 compose

```
function compose(...fns) {
      return function (x) {
        return fns.reduce(function (arg, fn) {
          return fn(arg);
        }, x)
      }
    }
    function a(num) {
      return num + 5
    }
    function b(num) {
      return num + 2
    }
    console.log(compose(a, b)(5))
```
