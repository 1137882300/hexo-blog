---
title: 开源媒体资源下载神器-> cobalt
shortTitle: 开源媒体资源下载神器-> cobalt
categories:
  - [开源项目]
  - [GitHub]
tags:
  - 开源项目
  - github
  - download
  - video
description: 开源媒体资源下载神器-> cobalt
date: 2024-09-28
keywords: 'cobalt,github,开源项目,video,download'
cover:  
abbrlink: 112261
---

> Cobalt 是一个没有广告、跟踪器的开源、免费在线下载工具。 你只需粘贴链接即可获取文件，无需任何复杂操作，媒体下载就该如此。
> cobalt 是一个不会让人烦恼的媒体下载器。它速度快，友好，并且没有互联网充斥着的内容：广告、跟踪器和付费墙。
> 同时，作者申明用户对内容的下载、使用和分发承担所有责任，并应完全、始终遵守内容许可协议

## 项目地址

✅Github仓库-[imputnet/cobalt](https://github.com/imputnet/cobalt)

✅官方链接-[cobalt](https://cobalt.tools/)

## 项目介绍

![https://appscross.com/wp-content/uploads/2024/09/homepage.webp](https://appscross.com/wp-content/uploads/2024/09/homepage.webp)

cobalt homepage

Cobalt 如此干净、纯粹、简洁，粘贴链接、获取文件、继续，三次点击，就这么简单。官方实例实测支持 YouTube、Instagram、Facebook、X/Twitter 、Twitch和 tiktok、bilibili 等平台视频下载（很多二把刀将官方实例支持的平台理解为项目支持的平台），如何实现官方宣称支持的所有平台媒体资源下载呢？答案就是私有化部署。为什么？懂的都懂。

![https://appscross.com/wp-content/uploads/2024/09/supported-media.webp](https://appscross.com/wp-content/uploads/2024/09/supported-media.webp)

supported platform

在 cobalt 站点，点击左侧的『Settings』，可以打开、编辑所有的设置项。其中，『video』选项可以选择下载视频的默认分辨率、编码格式等。

![https://appscross.com/wp-content/uploads/2024/09/settings.webp](https://appscross.com/wp-content/uploads/2024/09/settings.webp)

Cobalt 支持的平台有可能受到限制或下载失效，用户可以自定义处理服务器。在『Settings』中点击『Instance』，打开『use a custom processing server』开关后，输入自定义处理服务器即可。老E提醒，使用自定义处理服务器（私有化部署）务必注意知识产权问题。

> cobalt will use a custom processing server if you choose to. even though cobalt has some security measures in place, we are not responsible for any damages done via a community instance, as we have no control over them. please be mindful of what instances you use and make sure they're hosted by people you trust.
>

![https://appscross.com/wp-content/uploads/2024/09/process-sever.webp](https://appscross.com/wp-content/uploads/2024/09/process-sever.webp)

process server

Cobalt 是仅仅是一种简化从互联网下载内容的工具，用户对内容的下载、使用和分发承担所有责任。用户应完全、始终遵守内容许可协议，引用时必须注明内容原创者，合理使用和署名对每个人都有好处。Cobalt 采用在线下载方式，好处是理论上用户可以摆脱代理（可直连 cobalt 的前提下）下载 youtube、X、tiktok 等平台上的公开视频，但是，下载链接的获取、自定义处理服务器的应用，仍然需要代理。作者这里郑重推荐朋友的机场「[嘀嗒云](https://appscross.com/go/dida)」，订阅轻量套餐低至 ￥5/月，低价不低质、绝对便宜够用好用。

普通用户直接使用 cobalt 的官方实例（网站）在线下载就可以了，最新版本的 cobalt 可以自定义处理服务器，由此可知 cobalt 支持私有化部署，后文即分享 cobalt 的私有化部署。Cobalt 项目托关于github，项目仓库及官方实例站点如下：

## 个人部署
在默认已安装 docker、docker-compose 的前提下，首先，创建 cobalt-api 服务定义配置文件。
```shell
mkdir /opt/cobalt
cd /opt/cobalt
nano docker-compose.yml
```
docker-compose.yml 文件内容参考如下，可直接拷贝并修改 URL 使用。本配置中的两项服务 cobalt-api 和 watchtower，watchtower 服务配置块为可选，可整体删除。watchtower 默认监听端口为 8080，可通过环境变量修改 。

```shell
services:
    cobalt-api:
        image: ghcr.io/imputnet/cobalt:latest
        restart: unless-stopped
        container_name: cobalt-api
        init: true
        ports:
            - 9000:9000/tcp
        environment:
            API_URL: "http://cobalt.example.com/"
        labels:
            - com.centurylinklabs.watchtower.scope=cobalt
    watchtower:
        image: ghcr.io/containrrr/watchtower
        restart: unless-stopped
        command: --cleanup --scope cobalt --interval 900 --include-restarting
        volumes:
            - /var/run/docker.sock:/var/run/docker.sock
```
之后，在 cobalt 目录下启动容器。
```shell
docker compose up -d
```
缺失 docker compose 命令时，应安装 docker-compose-plugin。要停止并删除 cobalt 容器以及 cobalt 自定义网络，可使用如下命令。
```shell
docker compose down
#docker-compose rm -f
#docker rm -f $(docker ps -a)
docker network prune -y
```
同时，需要在 cobalt.tools 中打开『Settings』，在『Instance』下启用『use a custom processing server』，输入自定义的服务端（api 容器）URL 并提交。前文直接使用官方站点在线下载峰值速度可达 30Mbps，私有化部署的下载速度取决于 api 服务部署主机和下载端的网络环境。

![https://appscross.com/wp-content/uploads/2024/09/self-host.webp](https://appscross.com/wp-content/uploads/2024/09/self-host.webp)

### 完整私有化部署 

完整私有化部署要分别定义 api 和 web 服务，也就是需要加载两个容器。过程与使用最新版本部署 api 完全相同，仅服务定义文件 docker-compose.yml 有所区别。编辑适合自身网络环境和需求的服务定义文件时，应注意以下几点：

- IP 或域名均可使用，但 API IP 必须为公网 IP ，支持 IPv6
- WEB IP 如无公网访问需求，可为内网 IP
- 注意修改镜像名称，cobalt 仅部分版本提供适配 armv7、arm64 平台的镜像
- 支持 http、https，建议使用 https，仅私人访问可使用自签证书
- 建议使用域名而非 IP，并提前配置好 DNS 解析
- 如果部署于境外主机，建议使用 nginx、caddy 等进行反向代理，定义文件中应去掉端口号
- 最新版本 10 部署 api，混搭版本 7 部署 web 未经验证，谨慎使用

```shell
version: '1.0'
services:
    cobalt-api:
        image: ghcr.io/imputnet/cobalt:7
        restart: unless-stopped
        container_name: cobalt-api
        init: true
        tty: true
        ports:
            - 9000:9000/tcp
        environment:
            API_URL: "http://public_ip_or_domainname:9000/"
            API_NAME: "cobalt-api"
    cobalt-web:
        image: ghcr.io/imputnet/cobalt:7
        restart: unless-stopped
        container_name: cobalt-web
        init: true
        tty: true
        ports:
            - 9001:9001/tcp
        environment:
            WEB_URL: "http://ip_or_domainname:9001/"
            API_URL: "http://public_ip_or_domainname:9000/"
```

编辑好 docker-compose.yml 后，运行 docker compose up -d 命令，docker 将自动拉取镜像，并按照定义文件创建 cobalt-api、cobalt-web 两项服务。

![https://appscross.com/wp-content/uploads/2024/09/docker-compose.webp](https://appscross.com/wp-content/uploads/2024/09/docker-compose.webp)

docker compose

容器加载后，我们就可以通过域名或 IP 地址访问前端 web，并正常下载视频资源。示例使用了 http 协议，并添加 DNS AAAA 记录指向内网盒子。

![https://appscross.com/wp-content/uploads/2024/09/self-hosted-armbian.png](https://appscross.com/wp-content/uploads/2024/09/self-hosted-armbian.png)


> 最后，特别提示，无论使用何种下载工具下载何种资源，务必尊重版权，严格遵守内容协议、遵照原创声明对每个人都有好处。 