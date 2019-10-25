---
title: 常用的工具
date: 2019-07-15 17:09:46
tags: JS
---

### 超出行隐藏

```
overflow:hidden;
text-overflow:ellipsis;
display:-webkit-box;
-webkit-box-orient:vertical;
-webkit-line-clamp:2;   参数可为任意行数
```

### 实现一个 compose

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

### 去掉滚动条

```
.element::-webkit-scrollbar {display:none}
```

### flex 一些经常使用的方法

##### flex-direction:主轴的方向

- row（默认值）：主轴为水平方向，起点在左端。
- row-reverse：主轴为水平方向，起点在右端。
- column：主轴为垂直方向，起点在上沿。
- column-reverse：主轴为垂直方向，起点在下沿。

##### align-content 定义了多根轴线的对齐方式

- flex-start：与交叉轴的起点对齐。
- flex-end：与交叉轴的终点对齐。
- center：与交叉轴的中点对齐。
- space-between：与交叉轴两端对齐，轴线之间的间隔平均分布。
- space-around：每根轴线两侧的间隔都相等。所以，轴线之间的间隔比轴线与边框的间隔大一倍。
- stretch（默认值）：轴线占满整个交叉轴。

### 一些判断

```
let userAgent = ''
if (typeof navigator !== 'undefined') {
  userAgent = navigator.userAgent.toLowerCase()
}
// ios浏览器
export const isiOS = /applewebkit/i.test(userAgent)

// 微信浏览器
export const isWx = /micromessenger/i.test(userAgent)

// 支付宝
export const isAliPay = /alipayclient/.test(userAgent)

/**
 * @desc   判断是否pc页面
 * @return {Boolean}
 */
export const isPc = !/Android|webOS|iPhone|iPod|BlackBerry/i.test(userAgent)

/**
 * @desc   判断是否为手机号
 * @param  {String|Number} str
 * @return {Boolean}
 */
export function isPhone(str) {
  return /^(0|86|17951)?1[3456789]\d{9}$/.test(str)
}

/**
 * @desc   判断是否为邮箱地址
 * @param  {String}  str
 * @return {Boolean}
 */
export function isEmail(str) {
  return /\w+([-+.]\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*/.test(str)
}

/**
 * @desc   判断是否是数字
 * @param  {String|Number} str
 * @return {Boolean}
 */
export function isNum(str) {
  return /^[0-9]+([.]{1}[0-9]+){0,1}$/.test(str)
}

/**
 *
 * @desc  判断是否为身份证号
 * @param  {String|Number} str
 * @return {Boolean}
 */
export function isIdCard(str) {
  return /^(^[1-9]\d{7}((0\d)|(1[0-2]))(([0|1|2]\d)|3[0-1])\d{3}$)|(^[1-9]\d{5}[1-9]\d{3}((0\d)|(1[0-2]))(([0|1|2]\d)|3[0-1])((\d{4})|\d{3}[Xx])$)$/.test(
    str
  )
}
```

###
