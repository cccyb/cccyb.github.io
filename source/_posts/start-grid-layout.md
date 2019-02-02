---
title: 初探Grid-Layout布局
date: 2018/08/08 12:46:25
categories:
- Tutorials
tags:
- CSS
- Grid
- Layout
thumb: https://ws1.sinaimg.cn/large/c542ee77ly1fzs85dtlxsj20jg0d0woq.jpg
---

# 初探Grid-Layout布局

## Grid布局简介

开始之前，我们先来看一张图：

![小草本](https://user-gold-cdn.xitu.io/2018/3/1/161dd2b9841e67fb?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

没错，这其实就是我们小时候写的小格子本本，其实它跟我们今天要讲的主题Grid布局非常类似，其实Grid布局就是它的升级加强版。

CSS网格布局（又称“网格”），是一种二维网格布局系统。

CSS在处理网页布局方面一直做的不是很好。一开始我们用的是table（表格）布局，然后用float(浮动)，position（定位）和inline-block（行内块）布局，但是这些方法本质上是hack，遗漏了很多功能，例如垂直居中。后来出了[flexbox(盒子布局)](https://link.juejin.im/?target=http%3A%2F%2Fpeale.cn%2F2016%2F11%2F30%2F2016_11_30_flex%2F%23more)，解决了很多布局问题，但是它仅仅是一维布局，而不是复杂的二维布局，实际上它们（flexbox与grid）能很好的配合使用。

Grid布局是第一个专门为解决布局问题而创建的CSS模块,2012年11月06日成立[草案](https://link.juejin.im/?target=https%3A%2F%2Fwww.w3.org%2FTR%2Fcss3-grid-layout%2F)。

Grid是一个趋势，grid-layout不是为了取代flex-layout，它是flex的补充。grid擅长二维布局，flex擅长一维布局。他们需要各司其职。

## Grid === Table2.0？

既然说grid布局是网格布局，那是不是grid布局就是table布局的2.0升级版呢？其实不然。

他们是有相同之处的。比如都是把元素排列成行和列。但是表格和grid的区别在于，表格是有内容结构的，不能很自由地在里面做布局。而grid内部元素可以自由设定位置，允许重叠和设定层级的样。

## 浏览器兼容性

既然要使用最新的css布局，那浏览器对grid布局的兼容性这个点是逃避不了的，那我们接下来就来看看grid布局的兼容性如何呢。

> 在将兼容性之前，介绍一个非常实用的网站，[https://caniuse.com](https://caniuse.com)，这个网站上面可以对我们用到的各种web相关的属性，包括html，css属性进行浏览器兼容性的查询。  

- Flex布局兼容性

![](https://s10.mogucdn.com/mlcdn/c45406/180810_4ck07e203b6hlb7ijcl35jkh992g3_2312x1110.png)

可以看到，我们现在用的最多的Flex布局的浏览器兼容性已经达到了一个非常高的比例——95%，说明在如今的前端开发环境下，如果对浏览器要求不是非常苛刻，基本可以非常愉快的使用Flex布局了。

- Grid布局兼容性

![](https://s10.mogucdn.com/mlcdn/c45406/180810_66jb69f0idk0k7ig40jbblliaa6k9_2330x1106.png)

从图中可以看到，Grid布局和前面的Flex布局相比起来，虽然没有那么高的兼容比例，但是，经过了6年的沉淀与发展，也已经达到了86%，相对来说也已经比较完备了。所以，如果你们的代码基本都是在常见的最新的浏览器上进行允许，不用兼容万恶的前端克星IE，可以在平时的开发中尝试使用体验一下最新的Grid布局。

## Grid和Flex对比

Grid与Flex布局的共同点是元素均存放在一个父级容器内，尺寸与位置受容器影响。最核心的区别是Flex布局使用单坐标轴的布局系统，而Grid布局中使用二维布局，使元素可以在二个维度上进行排列，如下图所示：

- flex-layout

![flex](https://www.w3.org/TR/css3-grid-layout/images/flex-layout.png)

- grid-layout

![grid](https://www.w3.org/TR/css3-grid-layout/images/grid-layout.png)

上面两张图片来自于w3c官方css规范对Grid布局的介绍中的一组对比图，我们可以看到，flex布局很明显的是一维布局，元素在容器中都是横向或者纵向进行排列，并不能跨越维度进行排列。

而grid布局相比于flex布局，很明显是二维布局，grid布局不仅可以在横向上像flex已经排列，某些子元素还可以跨越维度，同时可以在横向和纵向上进行布局。

### 一维 vs 二维

有这样一张图：

![](https://s10.mogucdn.com/mlcdn/c45406/180810_3ecg91kl5hfaaabbh2b51khjhk7fg_465x118.png)

上面这个布局，我们其实主要是在一个方向上即横线上布局，比如在`header`里放3个button，此时，我们其实使用flex布局是最佳方案，我们可以使用很少的代码来实现这些布局。

又比如有这样一张图：

![](https://s10.mogucdn.com/mlcdn/c45406/180810_89hcc0hf69e50hb7j77gka2j0445i_499x274.png)

我们看到，其实这个布局已经不单单是一个维度了，他同时在横向和纵向上都有布局，这种情况下，其实我们使用Grid布局会更加灵活，并且会使你的标签更坚定，代码更容易维护。

你也可以结合两者一起使用，在上面的例子中最完美的做法是使用Grid来布局页面，使用Flex去对齐header里面的内容。

### 内容优先 vs 布局优先

再者，其实这两种布局方式的另一个核心区别是Flex是以**内容**为基础，而**Grid**是以布局为基础，听起来有些抽象，我们来用一个实际的例子来看一下。

我们有这样一段header的HTML代码：

```html
<header>
	<div>Home</div>
	<div>Search</div>
	<div>Logout</div>
</header>
```

在没有使用任何布局时，他们是这样的：

![](https://s10.mogucdn.com/mlcdn/c45406/180810_7jh25ig17l7d4a9f399fjil5chcl4_673x161.png)

当我们给外部`header`容器添加一个`display: flex`之后，他们会漂亮的在一条线上。

```css
header {
	display: flex;
}
```

![](https://s10.mogucdn.com/mlcdn/c45406/180810_6fc6dc8j307ja00ja17i765232jfh_668x76.png)

为了让logout button在最右边，我们可以给他指定一个margin：

```css
header > div:nth-child(3) {
	margin-left: auto;
}
```

效果如下：

![](https://s10.mogucdn.com/mlcdn/c45406/180810_2gl52ecbh70060a8011gidl774cg3_670x77.png)

值得注意的是，让元素本身决定他放在哪里，我们除了`display: flex`之外没有添加任何东西。

这就是Flex和Grid的核心差别，当我们使用Grid来创建这个header时，这个差别会更加明显。

使用Grid来实现上面的header布局，有很多方法，我们这里用一种非常简单的去做，我们的Grid有十列，没一列都是一个单位宽度。

```css
header {
	display: grid;
	grid-template-columns: repeat(10, 1fr);
}
```

添加上面的代码后，看起来其实和Flex的解决方案是一样的。

![](https://s10.mogucdn.com/mlcdn/c45406/180810_6fc6dc8j307ja00ja17i765232jfh_668x76.png)

但是我们可以使用chrome的审查元素在上帝视角来看看两者有什么不同：

![](https://s10.mogucdn.com/mlcdn/c45406/180810_58j3egaefjf9833691k1g4i83li54_748x138.png)

最关键的区别就是，这种方式必须先定义布局的列。从定义列的宽度开始，然后我们才能将元素放在可用的单元格中。这种方式强迫我们去分割我们的header有多少列。除非我们改变Grid，否则我们会被困死在这10列中，但是Flex中我们不会被这个麻烦困扰。

为了把logout放在最右边，我们会把他放在第十列：

```css
header > div:nth-child(3) {
	grid-column: 10;
}
```

审查元素时，看起来是这样的：

![](https://s10.mogucdn.com/mlcdn/c45406/180810_7lkl68d536gl00lja0fd44k4k3ij6_701x126.png)

我们不能简单的添加一个`margin-left: auto;`因为它引进被放在了第三个单元格中，想要移动它，我们得再找一个单元格把它放进去。

Grid和flex的区别，总结起来就是以下几点：

- CSS Grid适用于布局整体页面。它们使页面的布局变得非常容易，甚至可以处理一些不规则和非对称的设计。
- Flexbox非常适合对齐元素内的内容。你可以使用Flexbox来定位设计上一些较小的细节问题。
- CSS Grid适用于二维布局（行与列）。Flexbox适用于一维布局（行或列）。
- 同时学习它们，并配合使用。

## 重要术语

前面对Grid有了一个大概的了解后，我们来介绍以下Grid中比较重要的几个术语。

- 网格容器（grid-container）

网格容器，类似于Flex的容器，我们可以通过添加`display: grid`将一个元素设置成一个网格容器。比如下面这段代码中的`container`就是一个网格容器。

```html
<div class="conatiner">
	<div class="item item1">1</div>
	<div class="item item2">2</div>
	<div class="item item3">3</div>
</div>
```

- 网格项目（grid-item）

网格项目，就是网格容器中的一个子元素。比如下面代码中的`item`就是一个网格项目，但要注意，`sub-item`不是一个网格项目。

```html
<div class="conatiner">
	<div class="item"></div>
	<div class="item">
		<p class="sub-item"></p>
	</div>
	<div class="item"></div>
</div>
```

- 网格线（grid-line）

网格线就是将网格划分开的线条。

![](https://ws1.sinaimg.cn/large/c542ee77ly1fzs8bye7zrj20an06306x.jpg)

- 网格单元格（grid-cell）

网格单元格就是网格容器中划分出来最小的单元。

![](https://ws1.sinaimg.cn/large/c542ee77ly1fzs8cfc0uoj20an06306u.jpg)

- 网格轨道（grid-track）

网格轨道就是由若干个网格单元格组成的横向或者纵向区域，他的常见规格是1x8，或者8x1这种格式。

![](https://ws1.sinaimg.cn/large/c542ee77ly1fzs8csje9pj20an06306r.jpg)

- 网格区域（grid-area）

网格区域也是由若干个网格单元格组成的区域，但是不用与网格轨道，他的规格不局限与单个维度。

![](https://ws1.sinaimg.cn/large/c542ee77ly1fzs8dgkwdwj20an06306n.jpg)

## 基本属性

前面我们对grid布局的一些重要术语进行了介绍，接下来我们来一一介绍以下grid布局相关的基本属性。

#### 容器属性

容器属性，顾名思义，就是添加可以在网格容器中添加是属性，是对网格整体进行控制的一系列属性。

- display
- grid-template-columns
- grid-template-rows

这三个属性是用来定义网格布局最基本的三个属性，我们通过添加`display: grid`来设置一个网格容器，通过设置`grid-template-columns`和`grid-template-rows`来给网格容器定义具体的行和列。

```css
.container {
	display: grid;
	grid-template-columns: 40px 50px auto 50px 40px;
	grid-template-rows: 25% 100px auto;
}
```

![](https://ws1.sinaimg.cn/large/c542ee77ly1fzs8dyw8pxj20cw0a5a9u.jpg)

- fr单位

fr单位是grid布局中的一个新单位，它代表的是网格容器中**可用空间**的一份。下面我举三个小例子来介绍以下这个单位，注意，我们这里只关注列的宽度。

`1fr 1fr 1fr`表示三个轨道三等分。

![](https://s10.mogucdn.com/mlcdn/c45406/180810_396cg5l61dj1c7cf9gf3he9blidj8_619x418.png))

`2fr 1fr 1fr`表示三个轨道，空间四等分，两份给第一个轨道，剩下三个轨道各占一份。

![](https://s10.mogucdn.com/mlcdn/c45406/180810_6akc87d6hg2bhk3kldibc0g1b7d31_621x412.png)

`400px 2fr 1fr`表示三个轨道，第一个轨道400px，抽走400px后剩下空间三等分，两份给第二个轨道，一份给第三个轨道。

![](https://s10.mogucdn.com/mlcdn/c45406/180810_209b72gal4136kl69ag5ce105ck81_589x403.png)

- repeat()和minmax()

repeat和minmax是grid布局中的两个常用函数，可用减少我们代码的重复编写。

repeat(time, content)，表示的是标记重复部分或整个轨道列表，第一个参数time表示重复的次数，第二个参数content表示重新的内容。具体见下面的三个小例子。

```
repeat(3, 1fr) = 1fr 1fr 1fr

20px repeat(3, 1fr) 20px = 20px 1fr 1fr 1fr 20px

repeat(3, 1fr 2fr) = 1fr 2fr 1fr 2fr 1fr 2fr
```

minmax(min, max)，可用给网格定义一个尺寸的范围，第一个参数min表示网格尺寸的最小值，第二个参数表示网格尺寸的最大值。具体见下面的两个小例子。

```
minmax(100px, 200px)：表示网格最小是100px，最大是200px

minmax(100px, auto)：表示网格最小是100px，最大为auto，auto意思是行高将根据内容的大小自动变换
```

- grid-template-areas

grid-template-areas表示的网格容器中的一个区域。通过获取网格项中的grid-area属性值（名称），来定义网格模版。重复网格区（grid-area）名称将跨越网格单元格，’.’代表空网格单元。

下面这个例子我们通过给a，b，c，d四个div添加grid-area属性定义了名字，如果通过grid-template-areas这个属性来快速的定义网格的布局。

```css
.item-a {
	grid-area: header;
}
.item-b {
	grid-area: main;
}
.item-c {
	grid-area: sidebar;
}
.item-d {
	grid-area: footer;
}
.container {
	display: grid;
	grid-template-columns: 50px 50px 50px 50px;
	grid-template-rows: auto;
	grid-template-areas: 
		"header header header header"
		"main main . sidebar"
		"footer footer footer footer"
}
```

![](https://user-gold-cdn.xitu.io/2016/12/12/8b3d9c5cbdbba4375f2e4eee09107d18?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

- grid-column-gap
- grid-row-gap
- grid-gap

这三个属性，主要是用来定义网格项之间的间隙，类似于margin。grid-column-gap和grid-row-gap分别定义网格之间的列间距和行间距，而grid-gap则是简写，第一个值为行间距，第二个值为列间距。

```css
.container {
	grid-template-columns: 100px 50px 100px;
	grid-template-rows: 80px auto 80px;
	grid-column-gap: 10px;
	grid-row-gap: 15px;
}
```

或者

```css
.container {
	...
	grid-gap: 15px 10px;
}
```

![](https://user-gold-cdn.xitu.io/2016/12/12/57614742c974852d68a45aeb3b549335?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

- justify-items
- align-items
- justify-content
- align-content

这四个属性主要是用来控制网格项的对齐方式，具体用法和效果与Flex类似，这里就不详细展开，看图说话吧。

![](https://s10.mogucdn.com/mlcdn/c45406/180810_15h2gddfbed4i8gb01lj753j2ea5k_993x742.png)

![](https://s10.mogucdn.com/mlcdn/c45406/180810_6kjbdic8f2j5853111jl9g7dllh8e_978x778.png)

![](https://s10.mogucdn.com/mlcdn/c45406/180810_0ahbjl914lah4aaghcb5fk9i8de8d_1270x1004.png)

![](https://s10.mogucdn.com/mlcdn/c45406/180810_3fkc4990h47elf541cg3k05lc4ikg_1256x909.png)

- grid-auto-columns
- grid-auto-rows

这两个属性是自动生成隐式网格轨道（列和行），当你定位网格项超出网格容器范围时，将自动创建隐式网格轨道。

我们看下面这个例子。

```css
.container {
	display: grid;
	grid-template-columns: 60px 60px;
	grid-template-rows: 90px 90px;
}
```

![](https://user-gold-cdn.xitu.io/2016/12/12/c18240896c8ea2108874dad1c3db5c44?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

这是2x2的网格，但是我们来用grid-column 和 grid-row给网格项定位如下：

```
.item-a {
	grid-column: 1 / 2;
	grid-row: 2 / 3;
}
.item-b {
	grid-column: 5 / 6;
	grid-row: 2 / 3;
}
```

![](https://user-gold-cdn.xitu.io/2016/12/12/50645e52f1d44650e93ee2b410de7e65?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

我们可以看出，网格项item-b定位在第五根列网格线（column line 5 ）和第六根列网格线（column line 6 ）之间。但是我们网格容器根本不存在这两条网格线，所以就用两个0宽度来填充。在这里我们可以用网格自动行（grid-auto-rows）和网格自动列（grid-auto-columns）来定义这些隐式轨道宽度。

![](https://user-gold-cdn.xitu.io/2016/12/12/9bae2992a3f353fc0f1de9d44e20da3e?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

- grid-auto-flow

在没有设置网格项的位置时，这个属性控制网格项怎样排列。

他的属性值有：
```
row: 按照行依次从左到右排列。

column: 按照列依次从上倒下排列。

dense: 按先后顺序排列。
```

我们看下面的这个例子：

```html
<div class="container">
	<div class="item-a">item-a</div>
	<div class="item-b">item-b</div>
	<div class="item-c">item-c</div>
	<div class="item-d">item-d</div>
	<div class="item-e">item-e</div>
</div>
```

我们定义一个5行2列的网格，同时定义grid-auto-flow：flow。

```css
.container {
	display: grid;
  grid-template-columns: 60px 60px 60px 60px 60px;
  grid-template-rows: 30px 30px;
  grid-auto-flow: row;
}
```

然后对item-a和item-e进行布局。

```css
.item-a{
	grid-column: 1;
	grid-row: 1 / 3;
}
.item-e{
	grid-column: 5;
	grid-row: 1 / 3;
}
```

由于我们设置了grid-auto-flow：row，item-b、item-c和item-d在行上是从左到右排列，如下：

![](https://user-gold-cdn.xitu.io/2016/12/12/1514a1df1667618e3259fefcef9a8b9b?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

如果我们设置 grid-auto-flow: column;结果如下：

![](https://user-gold-cdn.xitu.io/2016/12/12/4b1c7ece0f66b1d6be3e6a82fad0a1c0?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

#### 网格项目属性

网格项目属性，是添加在具体的网格单元上来控制网格单元的属性。

- grid-column-start
- grid-column-end
- grid-row-start
- grid-row-end
- grid-column
- grid-row

这6个属性是通过网格线来定义网格项的位置。grid-column-start、grid-row-start定义网格项的开始位置，grid-column-end、grid-row-end定义网格项的结束位置。

这四个属性的值可以是：
```
line: 指定带编号或者名字的网格线。

span: 跨越轨道的数量。

span: 跨越轨道直到对应名字的网格线。

auto: 自动展示位置，默认跨度为1。
```

![](https://s10.mogucdn.com/mlcdn/c45406/180810_66h2dcgef69gg214bgg6k3h99hlf9_1312x715.png)

grid-column，grid-row是grid-column-start、grid-column-end 和 grid-row-start、grid-row-end 的简写。

![](https://user-gold-cdn.xitu.io/2016/12/12/d2bb8e2996cc62dd620642020463a2ca?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

- gid-area

定义网格项名字，以便创建模块（容器属性grid-template-areas来定义模块）。可以是数字或网格线名字。看如下两个例子。

定义网格项名字：

```css
.item-d {
	grid-area: header;
}
```

通过网格线定义网线项：

```css
.item-d {
	grid-area: 1 / col-start / last-line / 6;
}
```

![](https://user-gold-cdn.xitu.io/2016/12/12/e6c5321f34896104ac004ee45ac50e58?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

- justify-self
- align-self

这两属性用来定义单个网格项垂直于列网格线的对齐方式。

![](https://s10.mogucdn.com/mlcdn/c45406/180810_639hfc468ffighej44il14j5i4j11_1010x706.png)

![](https://s10.mogucdn.com/mlcdn/c45406/180810_7ekdll98jiada99247401bf6ehf38_1031x709.png)

### 实际布局应用

说了这么多，下面我们就拿几个常见的布局来应用一下刚刚学到的grid布局。

- 左右固定，中间自适应

```html
<div class="container">
	<div class="left">left</div>
	<div class="middle">middle</div>
	<div class="right">right</div>
</div>
```

```css
.container {
	display: grid;
	grid-template-columns: 100px 1fr 100px;
	height: 200px;
}
.container div {
	text-align: center;
}
.left {
	background: greenyellow;
}
.middle {
	background: lightblue;
}
.right {
	background: greenyellow;
}
```

![](https://s10.mogucdn.com/mlcdn/c45406/180811_1af7a6170cgac69gae8875988da5g_1786x510.png)

- 九宫格

```html
<div class="container">
	<div class="item">1</div>
	<div class="item">2</div>
	<div class="item">3</div>
	<div class="item">4</div>
	<div class="item">5</div>
	<div class="item">6</div>
	<div class="item">7</div>
	<div class="item">8</div>
	<div class="item">9</div>
</div>
```

```css
.container {
	display: grid;
	grid-template-columns: repeat(3, 1fr);
	grid-template-rows: repeat(3, 1fr);
	height: 400px;
	width: 400px;
	grid-gap: 8px;
}
.item {
	background: lightskyblue;
}
```

![](https://s10.mogucdn.com/mlcdn/c45406/180811_1ek17147c2075fj4g2d9d6gf980h8_938x944.png)

- 圣杯布局和双飞翼布局

```html
<div class="container">
	<div class="header">header</div>
	<div class="left">left</div>
	<div class="body">body</div>
	<div class="right">right</div>
	<div class="footer">footer</div>
</div>
```

```css
.container {
	display: grid;
	grid-template-columns: 100px 1fr 100px;
	grid-template-rows: 50px 300px 50px;
}
.header {
	grid-area: 1 / 1 / 2 / 4;
	background: lightsalmon;
}
.left {
	background: lightseagreen;
}
.body {
	background: lightslategray;
}
.right {
	background: lightyellow;
}
.footer {
	grid-area: 3 / 1 / 4 / 4;
	background: yellowgreen;
}
```

![](https://s10.mogucdn.com/mlcdn/c45406/180811_80ic9digjjg4hh0e2kd738fgllj6l_1718x620.png)

## 结束语

但是也不要放弃flex-layout，它是目前为止最厉害的页面布局属性，是时代召唤的结果，只是它并不适合布局整个页面框架。flex在响应式布局中是很关键的，它是内容驱动型的布局。不需要预先知道会有什么内容，可以设定元素如何分配剩余的空间以及在空间不足的时候如何表现。显得较为强大的是一维布局的能力，而grid优势在于二维布局。这也是他们设计的初衷。

大概可以设想，网格布局被广泛支持之后会出现很多网格布局内嵌flex的布局情形。

### 参考链接

- [https://www.w3.org/TR/css-grid-1/](https://www.w3.org/TR/css-grid-1/)
- [https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Grid_Layout](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Grid_Layout)
- [https://caniuse.com/#feat=css-grid](https://caniuse.com/#feat=css-grid)
- [http://griddy.io/](http://griddy.io/)
- [https://alialaa.github.io/css-grid-cheat-sheet/](https://alialaa.github.io/css-grid-cheat-sheet/)
- [http://www.w3cplus.com/css3/playing-with-css-grid-layout.html](http://www.w3cplus.com/css3/playing-with-css-grid-layout.html)

> 本文部分内容参考自其他相关文章，若有冒犯，请联系我删除。  

#蘑菇街