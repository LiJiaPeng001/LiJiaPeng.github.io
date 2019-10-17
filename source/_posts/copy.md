---
title: 复制粘贴功能
date: 2019-06-05 21:04:49
tags: JS
---

```
function copyTextToClipboard(text) {
    var textArea = document.createElement("textarea")

    textArea.style.position = 'fixed'
    textArea.style.top = 0
    textArea.style.left = 0
    textArea.style.width = '2em'
    textArea.style.height = '2em'
    textArea.style.padding = 0
    textArea.style.border = 'none'
    textArea.style.outline = 'none'
    textArea.style.boxShadow = 'none'
    textArea.style.background = 'transparent'
    textArea.value = text

    document.body.appendChild(textArea)

    textArea.select()

    try {
      var msg = document.execCommand('copy') ? '成功' : '失败'
      alert('复制内容 ' + msg);
    } catch (err) {
      alert('不能使用这种方法复制内容');
    }

    document.body.removeChild(textArea)
}
```

#### copyTextToClipboard('新垣结衣')
