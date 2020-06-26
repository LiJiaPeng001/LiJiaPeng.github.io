---
title: downLoad
date: 2020-06-26 11:20:05
tags: JS
---
```
class DownLoad {
    constructor(type) {
        this.arr = {}
        this.type = type
        this.arr[type] = 0
    }
    int() {
        this.arr[this.type] += 1
        this.total()
    }
    total() {
        let num = Object.entries(this.arr).reduce((all, [key, value]) => {
            all += value
            return all
        }, 0);
        console.log('总数量：', num)
        return num
    }
}
class DownLoadSmall {
    constructor(res = { isDown: false, url: './1.png' }) {
        this.arr = res
    }
    start(DownLoad) {
        if (this.arr.isDown) return this.arr
        return new Promise((resolve) => {
            setTimeout(() => {
                this.arr.isDown = true
                DownLoad.int()
                resolve(this.arr)
            }, 500)
        })
    }
}
class Image {
    constructor(images) {
        this.images = images
    }
    startDown(DownLoad) {
        return new Promise(async resolve => {
            for (let i = 0; i < this.images.length; i++) {
                let data = new DownLoadSmall(this.images[i])
                this.images[i] = await data.start(DownLoad, this.images[i])
            }
            console.log(this.images)
            resolve()
        })
    }
}

let images = [
    { isDown: false, url: './1.png' },
    { isDown: false, url: './2.png' },
    { isDown: false, url: './3.png' },
    { isDown: false, url: './4.png' },
    { isDown: false, url: './5.png' },
    { isDown: false, url: './6.png' },
    { isDown: false, url: './7.png' },
]
async function startDownImage() {
    let imgClass = new DownLoad('image')
    let img = new Image(images)
    await img.startDown(imgClass)
    await img.startDown(imgClass)
}
startDownImage()

```