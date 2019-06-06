---
title: 关于bind，apply，call以及箭头函数的区别
date: 2018-12-27 11:46:58
tags: JS
---

### call,apply

不比比，直接上代码

```
function add(c, d) {
  return this.a + this.b + c + d;
}

var o = {a: 1, b: 3};

// 第一个参数是作为‘this’使用的对象
// 后续参数作为参数传递给函数调用
add.call(o, 5, 7); // 1 + 3 + 5 + 7 = 16

// 第一个参数也是作为‘this’使用的对象
// 第二个参数是一个数组，数组里的元素用作函数调用中的参数
add.apply(o, [10, 20]); // 1 + 3 + 10 + 20 = 34
```

在使用 call 和 apply 时候，如果传递给 this 的值不是一个对象，JavaScript 会尝试使用内部 ToObject 操作将其转换为对象。因此，如果传递的值是一个原始值比如 7 或 'foo'，那么就会使用相关构造函数将它转换为对象，所以原始值 7 会被转换为对象，像 new Number(7) 这样，而字符串 'foo' 转化成 new String('foo') 这样
如果你传的 context 就 null 或者 undefined，那么 window 对象就是默认的 context（严格模式下默认 context 是 undefined）

### bind

bind 是 es5 新加的，当你调用 bind 的时候，跟 call 和 apply 不同，他返回的是一个新函数，新函数 this 将永久地被绑定到了 bind 的第一个参数，无论这个函数是如何被调用的。

```
function f(){
  return this.a;
}

var g = f.bind({a:"azerty"});
console.log(g()); // azerty

var h = g.bind({a:'yoo'}); // bind只生效一次！
console.log(h()); // azerty

var o = {a:37, f:f, g:g, h:h};
console.log(o.f(), o.g(), o.h()); // 37, azerty, azerty
```

### 箭头函数

在箭头函数中，this 与封闭词法上下文的 this 保持一致。在全局代码中，它将被设置为全局对象

```
var globalObject = this;
var foo = (() => this);
console.log(foo() === globalObject); // true

//如果将this传递给call、bind、或者apply，它将被忽略。

// 接着上面的代码
// 作为对象的一个方法调用
var obj = {foo: foo};
console.log(obj.foo() === globalObject); // true

// 尝试使用call来设定this
console.log(foo.call(obj) === globalObject); // true

// 尝试使用bind来设定this
foo = foo.bind(obj);
console.log(foo() === globalObject); // true
```

最后一点，当函数作为对象里的方法被调用时，它们的 this 是调用该函数的对象。

```
var o = {
  prop: 37,
  f: function() {
    return this.prop;
  }
};

console.log(o.f()); // logs 37
```
