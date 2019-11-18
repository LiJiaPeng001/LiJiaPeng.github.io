---
title: 实现一个简单的瀑布流
date: 2019-11-18 17:25:07
tags: JavaScript
---

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
    <style>
      .container {
        position: relative;
      }
      .left,
      .middle,
      .right {
        display: inline-block;
        margin: 30px;
        width: 500px;
        height: auto;
        vertical-align: top;
      }
      img {
        width: 100%;
      }
    </style>
  </head>

  <body>
    <div class="container flex">
      <div class="left">
        <img src="./avatar.jpg" alt="" />
        <img src="./tutu.jpg" alt="" />
      </div>
      <div class="middle">
        <img src="./sister.jpg" alt="" />
        <img src="./tutu.jpg" alt="" />
      </div>
      <div class="right">
        <img src="./sister.jpg" alt="" />
      </div>
    </div>
  </body>
  <script>
    const el = ['./avatar.jpg', './sister.jpg', './tutu.jpg']
    const $ = dom => document.querySelector(dom)
    const left = $('.left')
    const middle = $('.middle')
    const right = $('.right')
    window.addEventListener('scroll', () => {
      const scrolltop =
        document.documentElement.scrollTop || document.body.scrollTop
      const scrollHeight = Math.max(
        document.body.scrollHeight,
        document.documentElement.scrollHeight
      )
      const screenHeight =
        document.documentElement.clientHeight || document.body.clientHeight
      judgeSize()
      if (scrollHeight - (screenHeight + scrolltop) <= 100) {
        for (let i = 0; i < 12; i++) {
          let img = Math.floor(Math.random() * 3)
          let dom = judgeSize()
          dom.innerHTML += `<img src="${el[img]}"></img>`
        }
      }
    })
    const judgeSize = () => {
      const leftHeight = left.offsetHeight
      const middleHeight = middle.offsetHeight
      const rightHeight = right.offsetHeight
      const obj = {
        [leftHeight]: left,
        [middleHeight]: middle,
        [rightHeight]: right
      }
      let arr = [leftHeight, middleHeight, rightHeight].sort((a, b) => a - b)
      return obj[arr[0]]
    }
  </script>
</html>

```
