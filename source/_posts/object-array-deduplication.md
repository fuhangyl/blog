---
title: 对象数组去重
comment: true
date: 2024-02-01 16:07:38
tags:
description: 对象数组去重
categories: JavaScript
keywords: 对象数组去重，对象数组，去重，JS
---

平时我们在做项目时，经常会遇到去重操作，那么当我们遇到一个对象数组时，如何去重呢？
```js
let list = [
  { name: 'fuhang', age: 23 },
  { name: 'fuhang', age: 23 },
  { name: 'tom', age: 18 }
]

let hash = {}

list = list.reduce((item, next) => {
  hash[next.name] ? '' : hash[next.name] = true && item.push(next);
  return item;
}, [])
console.log(list)
// [ { name: 'fuhang', age: 23 }, { name: 'tom', age: 18 } ]
```

解析：
1. reduce()方法对累加器和数组中的每个元素（从左到右）应用一个函数，将其减少为单个值。关于reduce的详细用法可参考MDN
2. 主要利用了一个空的hash对象来判断，是否已经有同属性的对象，如果有则不执行任何操作，如果没有，则push到存储的对象中去。
