---
title: Vue框架原理
date: 2022-10-16 01:00:25
tags:
---

> 今天来记录一下 vue 的响应式原理，直观上，就是数据变化时，视图自动更新。想要实现这个，需要以下几点：

- 视图上依赖了哪些数据 - 依赖收集
- 能感知数据的变化 - 数据劫持/代理
- 更新视图 - 发布订阅模式

## 数据劫持/代理

> 不同版本两个 API defineProperty 和 Proxy

```js
const observe = (data) => {
  Object.keys(data).forEach((key) => {
    let currentValue = data[key];

    if (typeof currentValue === 'object') {
      observe(currentValue);
      data[key] = new Proxy(currentValue, {
        set(target, property, value, receiver) {
          console.log('检测到数据设置变动', currentValue);
          return Reflect.set(target, property, value, receiver);
        },
      });
    } else {
      Object.defineProperty(data, key, {
        enumerable: true,
        configurable: false,
        get() {
          // console.log('检测到数据获取变动', currentValue);
          return currentValue;
        },
        set(newVal) {
          currentValue = newVal;
          console.log('检测到数据设置变动', currentValue);
        },
      });
    }
  });
};
```

## 依赖收集

> vue 的 template 的写法时，会经过编译，编译听上去很困难，其实就是字符串的正则和便利

```js
let template = `
    <div id="app">
        <span>{{name}}</span>
        <span>{{obj.age}}</span>
    </div>
`;
let data = {
  name: '1',
  obj: { age: 2 },
};
const compile = (templateString, data) => {
  let replaceText = (template, data) => {
    return template.replace(/\{\{(.*?)\}\}/g, (matched, placeholder) => {
      console.log(placeholder);
      return placeholder.split('.').reduce((prev, key) => {
        return prev[key];
      }, data);
    });
  };

  return replaceText(templateString, data);
};

console.log(compile(template, data));

// /\{\{(.*?)\}\}/g      ? 在其他限制符后面是，匹配模式是非贪婪模式
```

另外的一些指能如 v-model 就是判断字符串，做操作

## 发布订阅的模式

```js
const event = function () {
  this.subscribers = [];

  function add(func) {
    this.subscribers.push(func);
  }

  function emit(func) {
    this.subscribers.forEach((item) => {
      if (func === item.name) {
        item.handler();
      }
    });
  }
  this.add = add;
  this.emit = emit;
};

const event1 = new event();

const handler1 = (aa) => {
  console.log(aa);
};

event1.add({
  name: 'handler1',
  handler: () => {
    handler1(123);
  },
});

event1.add({
  name: 'handler2',
  handler: () => {
    handler1(456);
  },
});

event1.emit('handler2');
```
