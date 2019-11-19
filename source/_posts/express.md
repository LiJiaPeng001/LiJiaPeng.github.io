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

# router

index 入口文件

```
app.use('/', require('./router/index.js'))
```

router 下的 index

```
const express = require('express')
const router = express.Router()

router.get('/', (req, res) => res.send('我是主页'))

module.exports = router
```

# 连接数据库

```
mkdir mysql
cd mysql
touch index.js
yarn add mysql -S
```

安装后在 index.js 里边写入

```
var mysql = require('mysql')
var db
if (!db) {
  db = mysql.createPool({
    host: 'localhost',
    user: 'admin',
    password: '1',
    database: 'lijiapeng'
  })
}

module.exports = db

```

然后在路由文件引入

```
const express = require('express')
const router = express.Router()
const db = require('../mysql')

router.get('/', (req, res) => {
  db.query('select * from user', (err, data) => {
    return res.json(data)
  })
})

module.exports = router
```
