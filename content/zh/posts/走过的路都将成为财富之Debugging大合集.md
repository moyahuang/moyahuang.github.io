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

**⚠️[truffle develop]PSSecurityException**
date: 2020/12/11
description:

```
息，请参阅 https:/go.microsoft.com/fwlink/?LinkID=135170 中的 about_Execution_Policies。
所在位置 行:1 字符: 1
+ truffle develop
+ ~~~~~~~
    + CategoryInfo          : SecurityError: (:) []，PSSecurityException
    + FullyQualifiedErrorId : UnauthorizedAccess
```
solution: 更改开发者选项
1. windows搜索并打开开发者设置
2. 点击标签powershell的应用
问题解决

**⚠️[remix] smart contract deploy error**
date: 2020/12/18
description:
```
creation of Judge errored: Error encoding arguments: Error: invalid BigNumber string (argument="value", value="", code=INVALID_ARGUMENT, version=bignumber/5.0.8)
```
solution: 应该在部署时传入构造函数的参数
{{< img src="/images/screenshots/Snipaste_2020-12-18_20-07-53.jpg" title="Remix IDE截图">}}

**⚠️ [web3.js] Invalid JSON RPC response: undefined**
date: 2020/12/18
description:
```
Error: Invalid JSON RPC response: undefined
```
solution: 网络不存在，检查`HttpProvider`参数url地址（Ganache创建的私有区块链端口为7545）

[web3.js] BigNumber Error: new BigNumber() not a base 16 number
date: 2020/12/18
description: 
```
BigNumber Error: new BigNumber() not a base 16 number: 
    at raise (...\node_modules\bignumber.js\bignumber.js)
```
solution:

研究内容：访问控制通过确保授权用户对资源进行权限范围内的操作的一种技术手段，其目的在于实现数据开放的同时保证数据的安全和隐私性。计划基于以太坊智能合约设计并实现去中心化的访问控制机制，对其可行性、性能、隐私性等方面进行探索。
关键词：访问控制，区块链，智能合约


