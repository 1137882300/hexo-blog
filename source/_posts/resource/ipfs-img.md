---
title: 继telegra.ph失效后，搭建免费图床(超简单)
layout: post
shortTitle: 继telegra.ph失效后，搭建免费图床(超简单)
categories:
  - [免费]
  - [图床]
  - [ipfs]
tags:
  - 图床
  - ipfs
description:  
date: 2024-10-29
keywords: '图床，ipfs'
cover:  https://cdn.img2ipfs.com/ipfs/QmU2fbecRq5sFKKPXtuubjY3bHJrGbgBiMKB6Qm8N49sRf
top: 1
abbrlink:  112265
---

# 优势

- 免注册、免登录直接上传 100 兆内的小文件，而且空间无限
- 不限制文件格式，任意格式均可上传
- 下载没有广告，没有限速
- 使用去中心化技术，即使网站关闭依然可以下载
- 不会和谐内容，不会删除资源，只要有链接地址，换任意一个 ipfs 网关就能下载

# 地址

- [github仓库](https://github.com/jialezi/img2ipfs?tab=readme-ov-file)
- [ipfs文档](https://docs.ipfs.tech/install/ipfs-desktop/#macos)
- [客户端下载](https://github.com/ipfs/ipfs-desktop/releases)

# 上传图片的方式

- https://cdn.img2ipfs.com
- IPFS Desktop 客户端上传
- PicGo 客户端上传

# IPFS Desktop 上传步骤

- IPFS Desktop 文件 上传
- 复制 CID
- 拼接网关地址
- 形成完整图片URL

## 例如

- CID：bafybeickqjkaoln7zxmla7piynxpsmk3ldm7en2jlmtvkoipfal6utifue
- 网关：i2.img2ipfs.com
- 拼接：https://i2.img2ipfs.com/ipfs/bafybeickqjkaoln7zxmla7piynxpsmk3ldm7en2jlmtvkoipfal6utifue
- 可加上filename：?filename=xxx.jpg

# PicGo 客户端上传步骤

- [下载地址](https://github.com/Molunerfinn/PicGo/releases)
- 安装插件：web-uploader（作者：ZQian）
- 配置参数
    ```text
    图床配置名：img2ipfs 
    API地址：https://api.img2ipfs.org/api/v0/add 
    POST参数名：file 
    JSON路径：Url (注意这里的U是大写的U)
    ```

# 收集的网关

```text
cf-ipfs.com
183.252.17.149:82
ipfs.genenetwork.org
ipfs.fleek.co
ipfs.azurewebsites.net
ipfs.kaleido.art
ipfs.globalupload.io
ipfs.slang.cx
ipfs.adatools.io
gateway.originprotocol.com
ipfs.best-practice.se
ipfs.drink.cafe
ipfs.denarius.io        
crustwebsites.net
bin.d0x.to
ravencoinipfs-gateway.com
ipfs.smartholdem.io
infura-ipfs.io
```

# ipfs自动网关

```text
https://cdn.img2ipfs.com/ipfs/
```

## 使用方法

- 使用ipfs自动网关的域名，拼接路径出 https://cdn.img2ipfs.com/ipfs/QmRGwiZR4qTJhhpgyiWq7ahL7gCTWXNqtqDshxd84mivFt
- 然后浏览器访问ipfs自动网关的时候，ipfs自动网关会根据访问者的地区进行一次跳转，跳转到一个真实的ipfs网关，图片就打开了。

# 推荐美图

- [unsplash](https://unsplash.com/explore)