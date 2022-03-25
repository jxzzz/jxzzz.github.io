---
title: Docker 初体验
date: 2022-03-21 16:10:23
tags: Docker
---

## 通过 Docker 文档我学会了什么

**如何制作一个镜像**
在项目的根路径下需要一个 **Dockerfile** 文件

```Dockerfile
# syntax=docker/dockerfile:1

FROM node:12.18.1  # 设置一个 node12.18.1 环境

ENV NODE_ENV=production # 设置环境变量

WORKDIR /app           # 将当前工作环境目录设置为app

COPY ["package.json", "package-lock.json*", "./"]   # 将文件下的 "package.json", "package-lock.json*" 复制到 当前目录下也是是/app/下

RUN npm install --production  # 安装依赖

COPY . .   # 复制当前所有文件到目标路径下

CMD [ "node", "server.js" ]  # 执行 cmd 命令 "node", "server.js"
```

docker 会根据文件描述生成一个镜像。所以需要理解 **Dockerfile** 文件的语法。

```bash
$  docker build .
```

**如何运行一个镜像**

name : 镜像名称

```bash
$  docker run  [name]
```

我们通过镜像运行一个容器，里面如果是一个服务，我们需要在主机访问服务。这样直接访问是不行的，因为容器里的环境是独立的同主机处隔离状态。
我们需要将端口映射出来,其实就是加个参数。

```bash
$  docker run  -p host:port    [name]
```

## 还缺点什么

### 数据库

```bash
$ docker run \
$  -it --rm -d \
$  -p 8000:8000 \
$  -e CONNECTIONSTRING=mongodb://mongodb:27017/notes \  #  设置环境变量 数据库地址
$  [name]
```

## 当参数越来越多时，会很繁琐所以可以使用配置文件 **docker-compose.dev.yml**

```bash
version: '3.8'
services:

 notes:
  build:
   context: .
  ports:
   - 8000:8000
   - 9229:9229
  environment:
   - SERVER_PORT=8000
   - CONNECTIONSTRING=mongodb://mongo:27017/notes
  volumes:
   - ./:/app
  command: npm run debug

 mongo:
  image: mongo:4.2.8
  ports:
   - 27017:27017
  volumes:   # 变量 持久化数据
   - mongodb:/data/db
   - mongodb_config:/data/configdb
volumes:
 mongodb:
 mongodb_config:
```

## volumes

当你生成一个镜像时，你每次运行它，它都都会生成一个新的，干净的环境。
问题 1： 我想要保存我的数据。持久化数据就用到变量?
volumes ,他会在你生成容器的时候，把 volumes 挂载上

```bash
$ docker-compose up -f docker-compose.dev.yml
```

## 学习思路

- 多动手，多实验
- 理解 Docker 基础概念。
- 学习，配置文件的语法，
- 高级用法，先去理解场景，

## Docker 和虚拟机的关系

> 未完待续

## 引用

[Docker 文档](https://docs.docker.com/get-started/overview/)
