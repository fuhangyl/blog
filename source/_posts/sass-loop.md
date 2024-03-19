---
title: SASS循环控制指令
comment: true
date: 2024-03-13 11:19:07
tags:
description: SASS循环控制指令
categories: SASS
keywords: SASS，Sass，循环，@for，@while，@each
---

SASS中提供了三种循环控制指令：
1. @for
2. @while
3. @each

#### 1、@for循环指令
@for循环指令提供了2种方式：
```scss
@for $i from <start> through <end>
@for $i from <start> to <end>
```
* $i: 循环变量
* start: 循环起始值 
* end: 循环结束值

上面两种方式的区别是，关键字 through 表示**包括** end 这个值，而 to 则**不包括** end 这个值，看看下面这个例子：
```scss
// through 方式
@for $i from 1 through 3 {
  .item-#{$i} { 
    width: 100px * $i; 
  }
}
// to 方式
@for $i from 1 to 3 {
  .item-#{$i} {
    width: 100px * $i;
  }
}
```
对应的css样式代码：
```css
/* through 方式 */
.item-1 { width: 100px; }
.item-2 { width: 200px; }
.item-3 { width: 300px; }
/* to 方式 */
.item-1 { width: 100px; }
.item-2 { width: 200px; }
```

#### 2、@while循环指令
@while指令，只要后面的条件为true，则一直执行循环体，只到条件为false时，停止循环。

下面是@while指令的一个简单的应用：
```scss
$i: 4;
@while $i > 0 {
  .item-#{$i} { 
    width: 100px * $i; 
  }
  $i: $i - 1;
}
```
对应的css样式代码：
```css
.item-1 { width: 400px; }
.item-2 { width: 300px; }
.item-3 { width: 200px; }
.item-3 { width: 100px; }
```

#### 3、@each循环指令
@each指令和其它两种指令不同的是，@each不是基于变量循环的，而是基于列表循环。

@each 循环指令的形式：
```scss
@each $var in <list>
```
上面的list就是一个列表

具体可看下面这个实例：
```scss
$list: margin padding;
@each $item in $list {
  .item {
    #{$item}: 10px;
  }
}
```


