---
title: 初见Scroll-Snap
date: 2018/10/18 12:46:25
categories:
- Tutorials
tags:
- CSS
- Scroll Snap
thumb: https://ws1.sinaimg.cn/large/c542ee77ly1fzs85sr2kij20jg0ajtfz.jpg
---
## 什么是CSS Scroll Snap？

我们先看一下MDN里对scroll snap的解释：

![](https://s10.mogucdn.com/mlcdn/c45406/181019_64d78b0fga46dbf98cc8ce5fb2lkh_642x245.jpg)

英语不好的童鞋可以悄悄咪咪的使用百度翻译或者谷歌翻译帮个忙，这段话的大意应该就是：

>  CSS Scroll Snap是CSS的一个模块，它引入了滚动捕捉位置，可以在滚动操作完成后强制滚动容器的滚动窗口到我们指定的滚动位置。

看了还是一脸懵逼？没关系，我们先看个在线🌰（[https://codepen.io/airen/pen/EeYNwr](https://codepen.io/airen/pen/EeYNwr)），思考一下这个🌰里的滚动与我们平时写的滚动有什么区别呢？

没错，在使用了scroll snap属性后，元素滚动起来有了一种阻尼感，并没有原生滚动的那种顺滑感，并且可以精确定位到指定位置。

简单来说，就是CSS Scroll Snap（CSS 滚动捕捉）允许你在用户完成滚动后多锁定特定的元素或位置。

## 简介

开始之前，我们先介绍两个概念。

- 捕捉

学过 CAD 系列软件的同学可能很清楚，我们在移动一个对象时，对象总能够自动吸附在栅格线上，使得对象只能落在栅格上的确定位置上，这就是栅格捕捉。或者这样说，在一个普通的量尺上，规定你的画笔只能落在 1mm 和 2mm 的刻度上，而不能落在他们之间。

- 滚动捕捉

滚动捕捉，即在滚动时对滚动位置进行捕捉。

其实scroll snap的用法很简单，最最最基本的用法只要两行代码就可以实现，先在父元素上定义`scroll-snap-type`，然后在子元素上定义`scroll-snap-align`即可。

```html
<div class="container">
    <section class="child"></section>
    <section class="child"></section>
    <section class="child"></section>
    ...
</div>
```

```css
.container {
    scroll-snap-type: y madatory;
}
.child {
    scroll-snap-align: start;
}
```

是不是很简单，这里我们先对scroll snap属性有个大概的了解，后续我们会详细讲解。

![](https://s10.mogucdn.com/mlcdn/c45406/181019_0l24i1l0c1hk9ddi1d1fkfci35i4l_922x290.jpg)

上图是来自w3c标准里的一个图示，这个图其实非常形象了描述了滚动捕捉。

图中的红色区域即为可滚动容器的可视区域或叫捕捉视口。图片黄色框的地方被称为捕捉区域。我们上面设置的 scroll-snap-align 中指定了横轴捕捉点为中心位置。此时将在捕捉视口区域中心（红色虚线）以及捕捉区域中心（黄色虚线）形成捕捉点。

由此我们可以知道，要想形成捕捉，需要以下两个条件：

1. 是个可滚动的区域
2. 确定捕捉视口和捕捉区域的捕捉点

既然我们要使用CSS属性，那么了解它的流量器兼容性是必不可少的。

![](https://s10.mogucdn.com/mlcdn/c45406/181019_34cl4f6h459549l348i816a7000di_915x400.jpg)

自2016年推出CSS Scroll Snap以来，浏览器对它的支持有了显著的改善。Google 69+、Firefox、Edge和Safari都支持它的某些版本。但是现实其实还是很残酷的，整体浏览器对scroll snap的支持度并不是很高，只要50%多，不到60%，因此，这个属性我们在平时使用还是需要谨慎考虑兼容性，使用最新的浏览器进行尝鲜未尝不可。待后续各大浏览器稳定支持后再进行扩展。

## 属性

与任何属性一样，熟悉它们所接受的值是一个好主意。滚动捕捉属性应用于父元素和子元素，每个元素都有特定的值。就像Flexbox和Grid那样，父类变成了Flex或Grid容器。在这种情况下，父元素变成了快照（Snap）容器。

父容器属性主要有两个：

- scroll-snap-type

![](https://s10.mogucdn.com/mlcdn/c45406/181019_5b71f8hcbakj2ckdf4jglk0h1401g_796x544.jpg)

通过设置 scroll-snap-type 将一个滚动容器转变为一个滚动捕捉容器，并且可以控制捕捉的严格度，如果没有指定严格度，默认为非精确的捕捉（proximity）。

scroll-snap-type 支持设置两个参数，第一个为捕捉轴向，第二个参数为捕捉严格度，可省略。

proximity值与mandatory十分相似，只是没有那么严格。改变浏览器的大小或者增加内容区，它可能会(或者而不会)重新获取snap点，取决于它与snap点间的距离。

- scroll-snap-padding

默认情况下，内容会吸附到容器的边缘。你可以通过在容器上设置scroll-snap-padding属性来改变它。它遵循与常规padding属性相同的语法。如果你的布局中有可能妨碍内容的元素（比如固定的标题），那么这个属性就非常的有用。

子元素属性主要有三个：

- scroll-snap-align

![](https://s10.mogucdn.com/mlcdn/c45406/181019_7dkf7fig7i557lh15ike64fcfe9g4_731x451.jpg)

这些是相对于滚动方向的。如果是垂直滚动，start指的是元素的顶部边缘。如果是水平滚动条，它指的是左边缘。center和end遵循相同的原则。你可以为滚动条的不同方向设置不同的值，这两个值之间用空格分隔开。

- scroll-snap-margin

scroll-margin 则是指定了捕捉区域与捕捉元素之间的边距。例如对捕捉区域设置了 scroll-margin: 20px; 那么实际上生成的捕捉区域会比捕捉元素的尺寸大。 scroll-margin 的参数与我们常见的 margin 参数形式相同，同时也有 margin 一样的 scroll-margin-top 等属性。

- scroll-snap-stop

默认情况下，滚动捕捉仅在用户停止滚动时启动，这意味着用户可以在停止之前跳过多个捕捉点。

你可以在任何子元素上设置scroll-snap-stop: always来改变它。这会强制滚动容器在该元素上停止，然后用户可以继续滚动。

## 举几个🌰

-  垂直列表

要使用垂直列表与每个列表元素对齐，只需要几行CSS。首先，我们告诉容器沿骑垂直轴捕捉：

```css
.container {
    scroll-snap-type: y mandatory;
}
```

  然后，我们定义捕捉点。这里，我们指定每个列表元素的顶部将成为一个捕捉点：

```css
.child {
    scroll-snap-align: start;
}
```

在线预览效果见：[https://codepen.io/airen/pen/yxBMPa](https://codepen.io/airen/pen/yxBMPa)

- 水平滑块

为了制作一个水平滑块，我们告诉容器沿着它的`x`轴对齐。

```css
.container {
    scroll-snap-type: x mandatory;
}
```

 然后，我们告诉容器哪个点被捕捉。为了使图库居中，我们将每个元素的中心点定义为一个捕捉点。

```css
.child {
    scroll-snap-align: center;
}
```

在线预览效果见：[https://codepen.io/cccyb/pen/KGQdMz](https://codepen.io/cccyb/pen/KGQdMz)

- 垂直全屏

我们可以直接在`body`元素`y`轴上设置捕捉点：

```css
body {
    scroll-snap-type: y mandatory;
}
```

然后，我们将每个部分的大小设置和视窗一样大，并将顶部边缘定义为捕捉点：

```css
section {
    height: 100vh;
    width: 100vw;
    scroll-snap-align: start;
}
```

 在线预览效果见：[https://codepen.io/airen/pen/GXKWyL](https://codepen.io/airen/pen/GXKWyL)

- 水平全屏

这个案例与前面的垂直全屏类似，但在`x`轴上使用了捕捉点。

```css
body {
    scroll-snap-type: x mandatory;
   
}

section {
    height: 100vh;
    width: 100vw;
    scroll-snap-align: start;
}
```

在线预览见：[https://codepen.io/airen/pen/EeYWQb](https://codepen.io/airen/pen/EeYWQb)

- 2D图像网格

滚动捕捉可以同时在两个方向上工作。同样，我们可以直接在`body`元素上设置`scroll-snap-type`：

```css
.container {
    scroll-snap-type: both mandatory;
}
```

然后，将每个`.tile`左上角定义为一个捕捉点：

```css
.tile {
    scroll-snap-align: start;
}
```

[在线预览见：https://codepen.io/airen/pen/aaoJGq](https://codepen.io/airen/pen/aaoJGq)

## 总结

一旦浏览器可以稳定的支持CSS Scroll Snap属性时，对于触控设备产生的意义将十分重大。如，我们可以在next/previous之间快速查看画廊中的每一张图片。

使用CSS创建Scroll Snap意味着不再需要使用JavaScript或者导入一个多余的库定义滚动行为。并且Scroll Snap属于硬件加速，保证了可以在浏览器中平滑的执行。

## 参考链接

- [https://www.w3.org/TR/css-scroll-snap-1/#scroll-snap-stop](https://www.w3.org/TR/css-scroll-snap-1/#scroll-snap-stop)
- [https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Scroll_Snap](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Scroll_Snap)
- [https://mp.weixin.qq.com/s/0Duum2IKSpc5fPNXPAPf6Q](https://mp.weixin.qq.com/s/0Duum2IKSpc5fPNXPAPf6Q)
- [https://www.w3cplus.com/css/practical-css-scroll-snapping.html](https://www.w3cplus.com/css/practical-css-scroll-snapping.html)
- [https://caniuse.com/#search=scroll-snap](https://caniuse.com/#search=scroll-snap)

