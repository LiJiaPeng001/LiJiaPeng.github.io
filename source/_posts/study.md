---
title: 前端学习笔记
date: 2019-12-05 14:15:15
tags: JavaScript
---

# js:关于类型，有哪些你不知道的细节

undefined 是一个变量，而并非是一个关键字，这是 JavaScript 语言公认的设计失误之一，所以，我们为了避免无意中被篡改，我建议使用 void 0 来获取 undefined 值。
Undefined 跟 null 有一定的表意差别，null 表示的是：“定义了但是为空”。所以，在实际编程时，我们一般不会把变量赋值为 undefined，这样可以保证所有值为 undefined 的变量，都是从未赋值的自然状态。

String 有最大长度是 2^53 - 1

基于对象而不是面向对象
