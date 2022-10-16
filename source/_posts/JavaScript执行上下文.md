---
title: JavaScript执行上下文
date: 2022-10-16 14:28:27
tags: javasccript
---

> 正确的了解 JavaScript 执行上下文，有利于理解 JavaScript 的其他概念，如闭包，this 指向，提交机制。

执行上下文就是 JavaScript 就是代码执行时所在的抽象环境概念。
执行上下文总共有三中类型

- 全局上下文
- 函数执行上下文
- Eval 函数执行上下文 【eval 函数时，执行上下文，应不推荐使用，暂不讨论】

## 上下文

在创建阶段发生如下三件事

- 确定 this 的值
- 词法环境创建
- 变量环境创建

```js
ExectionContext = {
    ThisBinding = <this value>,
    LexicalEnvironment = {},
    VarialeEnvironment = {}
}
```

在这个阶段将会决定 this 执行谁，
在全局环境中 this 执行全局对象
在函数执行上下文中，this 指向取决于函数的调用方式。查看函数在那个环境中执行。

待续
