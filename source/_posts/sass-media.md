---
title: 使用SASS简化媒体查询
comment: true
date: 2024-02-22 17:23:08
tags:
description: 使用SASS简化媒体查询
categories: SASS
keywords: SASS，Sass，媒体查询
---

我们平时开发时，会经常使用媒体查询来设置不同屏幕尺寸下的样式。于是就会下面的代码，不仅麻烦，而且不够直观，维护起来也很麻烦。

```css
@media (min-width: 320px) and (max-width: 480px) {
  .class1 {
    /* styles */
  }
  .class2 {
    /* styles */
  }
}
@media (min-width: 481px) and (max-width: 768px) {
  .class1 {
    /* styles */
  }
  .class2 {
    /* styles */
  }
}
```

其实我们可以使用SASS来简化媒体查询的写法。

首先我们需要定义一个变量，用于存储常用屏幕尺寸的范围。

```scss
// 定义媒体查询对象，防止变量在其他地方使用，这里定义为私有变量($后面添加-或者_)
$_breakpoints: (
  'phone': (320px, 480px), 
  'pad': (481px, 768px),
  'notebook': (769px, 1024px),
  'desktop': (1025px, 1200px),
  'tv': 1201px
);
```

接下来，我们定义一个mixin，用于生成媒体查询代码。

```scss
@mixin respond-to($breakname) {
  // 1、根据 $breakname 获取屏幕尺寸范围，这里可以使用 sass 中的 map-get 函数
  $bp: map-get($_breakpoints, $breakname);
  // 2、判断 $bp 的类型（从上面定义的变量可以看出，前四种为list类型，最后一种为非list），获取变量的类型可以使用 sass 中的 type-of 函数
  @if type-of($bp) == 'list' {
    // 3、获取 $bp 变量中的值，这里可以使用 sass 中的 nth 函数
    $min: nth($bp, 1);
    $max: nth($bp, 2);
    //  4、生成媒体查询代码
    @media (min-width: $min) and (max-width: $max) {
      // 5、在媒体查询代码中使用 @content 指令，将后面的代码插入到媒体查询代码中
      @content;
    }
  }
  @else {
    // 4、生成媒体查询代码（同上面的第四步），唯一的区别是这里的 $bp 不是list类型了，可以直接使用
    @media (min-width: $bp) {
      // 5、在媒体查询代码中使用 @content 指令，将后面的代码插入到媒体查询代码中
      @content;
    }
  }
  
}
```

至此，我们的 mixin 已经定义好了，接下来我们就可以在需要使用媒体查询的地方使用它了，记住不要忘记导入对应的文件了。

```scss
.class1 {
  @include respond-to('phone') {
    /* styles */
  }
  @include respond-to('pad') {
    /* styles */
  }
}
.class2 {
  @include respond-to('phone') {
    /* styles */
  }
  @include respond-to('pad') {
    /* styles */
  }
}
```

假如后面，媒体查询的尺寸需要修改，只需要修改 $_breakpoints 变量就可以了。不需要在到处一个个修改，大大降低了维护成本。

并且使用这种方式，非常直观的可以看出对应 className 在不同尺寸下的样式。
