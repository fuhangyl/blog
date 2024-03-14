---
title: CSS-文字穿透效果
comment: true
date: 2024-03-14 09:14:26
tags:
description: CSS-文字穿透效果
categories: 特效
keywords: CSS，css，文字穿透，效果
---

#### 1、实现效果
![CSS-文字穿透效果](../images/text_penetration/01.png)

#### 2、实现原理

html部分比较简单，只需要一个遮罩层的div容器，然后在容器内添加H1标签即可。
```html
<body>
  <div class="modal">
    <h1 class="text">学而不思则罔</h1>
  </div>
</body>
```

下面我们来拆解一下效果实现的步骤：
1. 首先要将body的宽高设置为100%，然后将自己喜欢的图片设置为背景图；
2. 接着将modal元素的宽高也设置为100%，并设置背景色，这里设置的值为rgba(0, 0, 0, 0.5)；
3. 然后将H1标签的宽高也设置为100%，因为后面要给H1标签添加背景图，同时设置文字水平垂直居中；
4. 最后我们就要设置文字的穿透效果了，具体实现，代码中会具体标注；

#### 3、实现源码
```scss
%container {
  width: 100%;
  height: 100%;
}
%bgImage {
  background-image: url('背景图片路径');
  background-size: cover;
}
body {
  @extend %container;
  @extend %bgImage;
}
.modal {
  @extend %container;
  background: rgba(0, 0, 0, 0.5);
}
.text {
  @extend %container;
  @extend %bgImage;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 12vw;
  // 设置描边
  text-stroke: 1px #fff;
  // 背景被裁剪成文字的前景色
  background-clip: text;
  // 设置文字透明
  color: transparent;
}
```
[效果传送门](https://codepen.io/fuhangyl/pen/WNWGxvx)

#### 4、总结
该效果实现主要应用了**background-clip**设置元素的背景图裁剪方式，具体实现可以参考[MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-clip)文档。