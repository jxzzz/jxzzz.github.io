---
title: 搭建博客过程
date: 2022-03-19 21:31:45
tags: Hexo Github
---

### 开始的地方

搭建博客是我刚学前端时的入门学习资料，前些时间发现之前的 blog，感觉很可惜，没能坚持下去。到如今已经工作四五年了，想重新拾起。
虽然是很简单的，还是搞了很长时间啊。汗颜啦 😓

## 预期目标

搭建一个简单的博客 [Jx's blog](https://imxue.github.io/)，很简单的功能

- [x] 归档
- [x] 关于
- [x] 标签

## 前期准备

- 基础的 git ，node，github 使用
- 本地 git + node 环境
- 在 Github 新建一个名为 **[userName].github.io** 仓库
- [可选] 选择一个[主题](https://hexo.io/themes/)，我选择的是 [polarbear](https://github.com/frostfan/hexo-theme-polarbear)

## 具体实施

### 安装 hexo-cli

```bash
$ npm install -g hexo-cli // 全局安装hexo-cli  // 当然也可以局部安装
```

### 初始化 hexo 项目

```bash
$ hexo init <folder> // 生成一个hexo 项目
$ cd <folder>
$ npm install // 安装依赖

$ git init // 初始git
```

### 添加主题

> 根据你选择的主题有不同的安装方式，看相关主题的使用说明。

hexo 项目中设置主题是在项目根路径下**\_config.yml** 文件中设置 **theme**字段。

在本文选择的[主题](https://hexo.io/themes/)中，需要安装两个依赖。

```bash
$ npm install hexo-renderer-scss hexo-renderer-swig --save
```

> 你可以 fork 下你主题的项目，到你的 githu 仓库里,这样修改起来方便些。

```bash
$ git submoudle add  https://github.com/imxue/hexo-theme-polarbear  themes/polarbear
```

> 这里有个 submodule(子模块) 的[概念](https://git-scm.com/book/en/v2/Git-Tools-Submodules),个人理解就是在一个 git 项目中包含另外一个 git 项目，主项目依赖里面的项目，但子项目其实是一个独立的项目。这样的好处是隔离便于管理。

**本地 hexo 项目应该已经完成**

在 Github 新建一个名为 **[userName].github.io** 空仓库
在本地仓库中添加

```bash
$  git remote add origin https://github.com/imxue/imxue.github.io.git
```

本地测试完好推送远程仓库

> SSH keys 应该有生成吧

```bash
$  git push origin master:master
```

### 添加 workflows

> 在 **.github/workflows/pages.yml** ，如果没有就新建一个

```yml
name: Pages

on:
  push:
    branches:
      - master # default branch
jobs:
  pages:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2

      - name: Checkout theme repo
        uses: actions/checkout@v2
        with:
          repository: imxue/hexo-theme-polarbear
          ref: master
          path: themes/polarbear

      - name: Use Node.js 14.x
        uses: actions/setup-node@v2
        with:
          node-version: '14'

      - name: Cache NPM dependencies
        uses: actions/cache@v2
        with:
          path: node_modules
          key: ${{ runner.OS }}-npm-cache
          restore-keys: |
            ${{ runner.OS }}-npm-cache

      - name: Install Dependencies
        run: npm install

      - name: Build
        run: npm run build

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
```

```bash
$  git push origin master:master
```

不出意外在你的 Settings/Code and automation/pages 页面上选择 gh-pages(自动生成的)分支 ，点 save 就看到 博客 地址

### 学习体会

现在的工程化做的都挺好， 一套下来 10 个命令应该可以搭建成功以个博客。
<br/>
我在这其中遇到很多问题，比如我刚开始看的是中文文档的 hexo，使用的是 CircleCI，搭建失败。会提示我超过用户最大数。
然后切换英文文档，之前都是很顺利的，就是在 deploy 总是说找不到主题文件。。然后我就必须要去理解，githubAction 做了什么，然后我查看 actions/checkout@v2，可能会解决我的问题。
<br/>
所以我感觉过程本身并不难，需要去理解原理。

### 引用

> 需要去阅读的资料

- [Hexo 文档](https://hexo.io/docs/)
- [Git 文档](https://git-scm.com/)
- [GitHubAction 文档](https://docs.github.com/cn/actions)
- [GithubPages 文档](https://docs.github.com/cn/pages/getting-started-with-github-pages)
