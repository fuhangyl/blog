---
title: map、parseInt组合考点
comment: true
date: 2024-02-19 13:51:31
tags:
description: map、parseInt组合考点
categories: JavaScript
keywords: map，parseInt，Array.map
---

['1', '2', '3'].map(parseInt) what & why ? 这题主要主要考了两个知识点：

#### 1. Array.map()
map方法会接受一个callback函数，callback 函数会被自动传入三个参数：
+ currentValue：callback 数组中正在处理的当前元素
+ index(可选)：callback 数组中正在处理的当前元素的索引
+ thisArg(可选)：执行 callback 函数时使用的 this 值

map方法最后返回一个经过callback计算后的数组，并且不会修改原数组。
[参考：map | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/map)

#### 2. parseInt()
parseInt方法接受两个参数：
+ string：要被解析的值。如果参数不是一个字符串，则将其转换为字符串(使用  toString 抽象操作)。字符串开头的空白符将会被忽略。
+ radix：一个介于2和36之间的整数(数学系统的基础)，表示上述字符串的基数。比如参数"10"表示使用我们通常使用的十进制数值系统。不同的基数会产生不同的结果，当未指定基数或基数为0时，则数字将以 10 为基础来解析。

**注意：** radix参数为n 将会把第一个参数看作是一个数的n进制表示，而返回的值则是十进制的。例如：
```js
// 将'123'看作5进制数，返回十进制数38 => 1*5^2 + 2*5^1 + 3*5^0 = 38
parseInt('123', 5)
```
[参考：parseInt | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/parseInt)

所以上题其实就是等价于：
```js
parseInt('1', 0)   // 1       基数为0时，已十进制解析
parseInt('2', 1)   // NaN     radi是介于2~36之间的数
parseInt('3', 2)   // NaN     2进制数中不会出现3
```

还可以看看下面这一题：
```js
[ '10', '10', '10', '10', '10' ].map(parseInt)
```

其实答案已经很明显了：
```js
parseInt('10', 0)     // 10
parseInt('10', 1)     // NaN
parseInt('10', 2)     // 2         1*2^1 + 0*2^0
parseInt('10', 3)     // 3         1*3^1 + 0*3^0
parseInt('10', 4)     // 4         1*4^1 + 0*4^0
```

有的时候我们还需要注意进制数的规则，比如下面的例子：
```js
parseInt('3', 2)   
// NaN   2进制数中是不允许出现大于等于2的数字的
parseInt('5', 4)   
// NaN   4进制数中是不允许出现大于等于4的数字的
parseInt('124', 4) 
// 6  4进制数中不允许出现大于等于4的数字，所以'124'会被转换为'12'，然后'12'转换为四进制 1*4^1 + 2*4^0 = 6
```
