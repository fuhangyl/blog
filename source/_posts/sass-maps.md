---
title: SASS Maps函数
comment: true
date: 2024-03-18 15:38:21
tags:
description: SASS Maps函数
categories: SASS
keywords: SASS，Sass，Maps，map-get，map-merge，map-keys，map-values，map-remove，map-has-key，keywords
---

#### 1、Maps数据类型
了解SASS中的Maps函数之前，我们首先要学习SASS中的Maps数据类型。

SASS Maps是一种类似于JavaScript对象的数据类型，它允许我们存储和操作键值对。
```scss
$map: (
  key1: value1, 
  key2: value2
)
// Maps也是可以嵌套的
$map1: (
  key1: value1,
  key2: (
    key2_1: value2_1,
    key2_2: value2_2      
  ),
  key3: value3      
)
```
Maps的嵌套在平时的开发中实用性还是比较高的，比如平时做后台管理系统时，会经常涉及到换肤功能：
```scss
$theme: (
  // 默认主题
  default: (
    /* styles */
  ),
  // 暗黑主题
  dark: (
    /* styles */
  )      
)
```

为了使Maps数据类型更加灵活，SASS还提供了Maps函数，允许我们动态地创建、修改和操作Maps。在SASS中Maps自身带了六个函数：
1. map-get($map,$key)：根据给定的key值，返回Maps中相关的值
2. map-merge($map1,$map2)：将两个Maps合并成一个新的Maps
3. map-remove($map,$key)：从Maps中删除一个key，返回一个新Maps
4. map-keys($map)：返回Maps中所有的key
5. map-values($map)：返回Maps中所有的value
6. map-has-key($map,$key)：根据给定的key值判断Maps是否有对应的value值，如果有返回true，否则返回false

#### 2、map-get($map,$key)函数
map-get函数可以根据给定的$key的值，返回$map中对应的value值，如果$map中没有对应的$key，则返回null。函数包含两个参数：

* $map:  定义的Maps数据
* $key:  需要遍历的key值

下面我们来看看map-get函数的用法：
```scss
$fonts: (
  default: 16px,
  small: 14px,
  large: 18px      
)
.font-default {
  font-size: map-get($fonts, default);
}
.font-small {
  font-size: map-get($fonts, small);
}
.font-large {
  font-size: map-get($fonts, large);
}
// $fonts中并没有custom对应的value值，最后会返回null
.font-custom {
  font-size: map-get($fonts, custom);
}
```
编译后的CSS代码：
```css
.font-default {
  font-size: 16px;
}
.font-small {
  font-size: 14px;
}
.font-large {
  font-size: 18px;
}
```
从编译后的css代码中可以发现，当$fonts中没有custom对应的value值时，会返回null，同时也不会编译在css代码中。

[map-get案例传送门](https://fuhangyl.github.io/sass-media)

#### 3、map-merge($map1,$map2)函数
map-merge函数可以将两个Maps合并成一个新的Maps，如果两个Maps中有相同的key，则后面的会覆盖前面的。
```scss
$map1: (
  color: red,
  font-size: 24px,
  background: #ffffff      
)
$map2: (
  line-height: 1.5, 
  font-weight: bold,
  background: #000000      
)
```
如果我们想将$map1和$map2合并成一个新的Maps，可以使用map-merge函数：
```scss
$map3: map-merge($map1, $map2)
```
我们将得到一个新的Maps：
```scss
$map3: (
  color: red,
  font-size: 24px,
  /* 后面的Maps($map2)会覆盖前面的Maps($map1) */  
  background: #000000,
  line-height: 1.5,
  font-weight: bold,      
)
```

#### 4、map-remove($map,$key)函数
map-remove函数可以删除Maps中的指定的$key，并返回一个新的Maps。
```scss
$map1: (
  color: red,
  font-size: 24px,
  background: #ffffff      
)
$map2: map-remove($map1, background)
```
我们将得到一个新的Maps：
```scss
$map2: (
  color: red,
  font-size: 24px
)
```

#### 5、map-keys($map)函数
map-keys函数可以获取Maps中的所有的$key，并返回一个列表。
```scss
$map: (
  margin: margin,
  padding: padding
)
$list: map-keys($map)
.box {
  @each $cls in $list {
    #{$cls}: 10px 20px;
  }
}
```
编译后的CSS代码：
```css
.box {
  margin: 10px 20px;
  padding: 10px 20px;
}
```

#### 6、map-values($map)函数
map-values函数和map-keys函数功能类似，不同的是map-values是获取Maps中的所有的$value，并返回一个列表，还有一点map-values会返回重复的$value。
```scss
$map: (
  marginL: 10px,
  marginR: 20px,
  marginT: 10px,
  marginB: 30px      
)
$list: map-values($map)
/* $list: 10px,20px,10px,30px */
```

#### 7、map-has-key($map,$key)函数
map-has-key函数返回一个布尔值。当$map中有这个$key，则函数返回true，否则返回false。开发时可以合理的利用此函数，来增强代码的健壮性。
```scss
$map: (
  margin: 10px,
  padding: 10px      
)
/* 如果有padding值，则设置对应的值 */
@if map-has-key($map, padding) {
  .box {
    padding: map-get($map, padding);
  }
}
/* 如果没有则设置默认值 */
@else {
  .box {
    padding: 20px;
  }
}
```
上述的案例中，如果没有使用map-has-key函数判断并且$map中没有padding值，则.box中不会生成任何css代码。
