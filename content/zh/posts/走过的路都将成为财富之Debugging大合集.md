---
title: "走过的路都将成为财富之Debugging大合集"
date: 2020-12-11T14:10:50+08:00
author: "Moya"
hideToc: false
enableToc: true
enableTocContent: false
author: Moya
authorEmoji: 🙀
tags:
- 踩坑
- 开发

draft: false
---
# Smart Contract
**⚠️[truffle init]Error: Error making request to https://raw.githubusercontent.com...**
date: 2020/12/11
description:
```
√ Preparing to download
× Downloading
Error: Error: Error: Error making request to https://raw.githubusercontent.com/truffle-box/bare-box/master/truffle-box.json. Got error: read ECONNRESET. Please check the format of the requested resource.
    at Object.unbox (C:\Users\w\AppData\Roaming\npm\node_modules\truffle\build\webpack:\packages\truffle-box\box.js:65:1)
    at processTicksAndRejections (internal/process/task_queues.js:97:5)
Truffle v5.0.2 (core: 5.0.2)
Node v12.18.3
```
solution: 修改hosts
1. 打开C:\windows\system32\drivers\etc
2. 在hosts文件中增加`199.232.68.133 raw.githubusercontent.com`

raw.githubusercontent.com的ip地址可以在以下或其他网站查询，没有则可以另外谷歌：
https://www.ipaddress.com/