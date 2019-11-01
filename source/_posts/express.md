---
title: express学习日记
date: 2019-11-01 11:14:07
tags: express
---

# hello,world

19.11.1
首先来一个 hello,world

```
mkdir express
cd express
npm init -y
yarn
yarn add express -S
yarn add nodemon -S
```

好了接下来写代码

```
const express = require('express')
const app = new express()

app.get('/', (req, res) => res.send('hello.world'))
app.listen(8888, () => console.log('我永远喜欢新垣结衣，8888'))
```

学习完毕，关电脑玩游戏（滑稽）
