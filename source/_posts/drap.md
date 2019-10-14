---
title: 实现一个简单的拖拽
date: 2019-10-14 17:10:48
tags:
---

#### 采用了 h5 的新特性

```
<!DOCTYPE html>
<html lang="en">

<head>
   <meta charset="UTF-8">
   <meta name="viewport" content="width=device-width, initial-scale=1.0">
   <meta http-equiv="X-UA-Compatible" content="ie=edge">
   <title>Document</title>
   <style>
      .el {
         flex-wrap: wrap;
      }

      .a,
      .b {
         width: 400px;
         height: 400px;
         border: 3px solid greenyellow;
         margin-right: 80px;
         margin-bottom: 50px;
         line-height: 400px;
         text-align: center;
         font-size: 56px;
      }

      .flex {
         display: flex;
      }
   </style>
</head>

<body>
   <div class="el flex">
      <div class="a" draggable='true'>1</div>
      <div class="b" draggable='true'>2</div>
   </div>
</body>
<script>
   let $ = (el) => document.querySelector(el)
   let $$ = (el) => document.querySelectorAll(el)
   class Drag {
      constructor(dom) {
         this.dom = dom
         this.fromDom = null
         this.toFrom = null
      }
      init() {
         let filterChild = [...this.dom.childNodes].filter(it => it.nodeName !== "#text" && !/\s/.test(it.nodeValue))
         filterChild.map(it => {
            it.setAttribute('draggable', 'true')
            this.dragStart(it)
            this.dragEnter(it)
            this.drop(it)
         })
      }
      dragStart(dom) {
         let that = this
         dom.ondragstart = function () {
            that.fromDom = dom
         }
      }
      dragEnter(dom) {
         let that = this
         dom.ondragover = function (e) {
            that.toFrom = dom
            e.preventDefault();
         }
      }
      drop(dom) {
         let that = this
         dom.ondrop = function (e) {
            let d = that.fromDom.innerHTML
            that.fromDom.innerHTML = that.toFrom.innerHTML
            that.toFrom.innerHTML = d
            that.fromDom = null
            that.toDom = null
         }
      }
   }
   let drap = new Drag($('.el'))
   drap.init()
</script>

</html>
```
