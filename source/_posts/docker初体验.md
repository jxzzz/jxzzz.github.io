---
title: Docker初体验
date: 2022-03-21 16:10:23
tags: Docker
---

## 我是谁

文档描述：

> Docker is an open platform for developing, shipping, and running applications.

Docker 是一个可以开发，交付 ，运行程序的平台。

## 我能干什么

## 基本概念

#### 镜像 images

需要在项目下拥有一个**Dockerfile**文件

```Dockerfile

# syntax=docker/dockerfile:1

FROM node:12.18.1
ENV NODE_ENV=production

WORKDIR /app

COPY ["package.json", "package-lock.json*", "./"]

RUN npm install --production

COPY . .

CMD [ "node", "server.js" ]
```

> 下载一个 node12.18.1 环境<br/>
> WORKDIR /app 设置当前路径为/app<br/>
> 复制 package.json", "package-lock.json\*", 到/app 下<br/>
> 运行 npm install --production <br/>
> COPY . . 复制所有当前目录下的问到镜像中 <br/>
> 执行 node server.js

```bash
$ docker build [name] .
```

#### 磁盘 volumes

#### 容器 container

## 基本操作

生成镜像 通过运行镜像来生成容器，使用 volumes 来持久化数据。

## 高级进阶

## 引用

[Docker 文档](https://docs.docker.com/get-started/overview/)
