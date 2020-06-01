---
title: "利用Docker-Compose构建TTRSS"
tags:
    - ttrss
    - TinyTinyRSS
    - Docker-Compose
    - RSS
---

> https://git.tt-rss.org/fox/ttrss-docker-compose

在这个信息爆炸的时代，RSS可以有效提高每日的消息获取速度和利用效率。

几方搜索，发现目前好用的RSS Client不是若干年前的就是早已停止支持的。

最后发现目前还在维护的一个开源的工具TinyTinyRSS，还可以使用。

ttrss使用PHP语言构建，持久层使用PostgreSQL，但不清楚该系统是否有漏洞，也暂时未对该工具源码进行审计。不建议搭建至公网，所以在自己内网的Docker中构建，这样即使出现问题，只要docker短期未出现逃逸，就不会影响内网安全（最坏就是此工具报废，重新构建）。

废话少说，开始构建：

在官网的git上发现有docker的构建方式，虽说不是封装好的image，但是也可以用docker-compose快速构建

开搞！

从git上clone compose部分构建脚本，随后进入目录

```bash
git clone https://git.tt-rss.org/fox/ttrss-docker-compose.git ttrss-docker && cd ttrss-docker
```

随后复制目录中的`.env-dist`文件到 `.env`，然后修改其中的DBpasswd

```bash
cp .env-dist .env && vim .env
```

随后开始构建

```bash
docker-compose up --build
```

开始经历漫长的等待

构建好了，发现自己的docker内部并不能直接请求外网，检查配置脚本，发现是创建好docker container后git clone。。。吐了

发现有一个分支为static-dockerhub，检查其脚本发现，嗯，这次的应该可以，先clone然后扔进去

checkout！

```bash
git checkout static-dockerhub
```

随后重新构建

```
docker-compose up --build -d
```

然后出现新的问题，rsync出现目录错误的问题

反馈官方，等待回复




