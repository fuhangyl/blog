---
title: SASS嵌套
comment: true
date: 2024-03-05 17:45:54
tags:
description: SASS嵌套
categories: SASS
keywords: SASS，Sass，选择器嵌套，属性嵌套，伪类嵌套
---

SASS中提供了三种嵌套方式：
1. 选择器嵌套
2. 属性嵌套
3. 伪类嵌套

#### 1、选择器嵌套
我们平时用的最多的就是选择器嵌套了，比如：
```scss
p {
  span {
    color: red;
    div & {
      color: blue;
    }
  }
}
```
对应的css样式代码：
```css
p span {
  color: red;
}
div p span {
  color: blue;
}
```

#### 2、属性嵌套
假如有下面一点css代码：
```css
div {
  font-size: 20px;
  font-weight: bold;
  font-style: italic;
}
```
我们可以使用属性嵌套来写成：
```scss
div {
  font: {
    size: 20px;
    weight: bold;
    style: italic;
  }
}
```


#### 3、伪类嵌套
伪类嵌套也是开发中经常用的一种嵌套方式，比如下面这段css代码：
```css
div:before, 
div:after {
  content: '';
  width: 10px;
  height: 10px;
}
div:before {
  background: red;
}
div:after {
  background: blue;
}
```
我们可以使用伪类嵌套来写成：
```scss
div {
  &:before,
  &:after {
    content: '';
    width: 10px;
    height: 10px;
  }
  &:before {
    background: red;
  }
  &:after {
    background: blue;
  }
}
```
平时开发中，我们可以合理的应用上述三种嵌套方式，来简化代码书写，提高开发效率。
