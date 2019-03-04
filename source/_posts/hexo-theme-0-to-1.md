---
title: 从零开始一个Hexo主题
date: 2019-02-26 11:38:38
categories:
- Tutorials
tags:
- Hexo
- Theme
thumb: https://ws1.sinaimg.cn/large/c542ee77ly1g0jogbl6ljj21z418gtln.jpg
---

### 前言

本文将会从零开始编写一个简单的Hexo博客主题，目的是了解一个Hexo博客主题的构成以及如何编写，因此，本示例中的博客页面样式不做过多描绘，样式主要参考 [Hexo theme](https://hexo.io/themes/) 中的 [Noise](https://github.com/lotabout/hexo-theme-noise) 主题。

在开始前，你需要对以下的一些知识点有必要的了解：

- 模板引擎语法
- CSS预处理器
- YML语法
- Hexo文档

本文使用的模板引擎为 [ejs](https://ejs.bootcss.com/)，使用的 CSS 预处理器为 [stylus](http://stylus-lang.com/)。这也是 hexo 项目预装了的 render 插件，如果想使用其他模板引擎或者其他 CSS 预处理器，可以安装相对应的 render 插件。

本文的代码：[https://github.com/cccyb/theme-example](https://github.com/cccyb/theme-example)

### 目录结构

根据下图，在`themes`目录下新建一个`theme-example`文件夹作为我们的主题目录。

![](https://ws1.sinaimg.cn/large/c542ee77ly1g0ipnb7mhgj20f407e0t6.jpg)

如图所示，一个hexo主题的目录主要包括以下五部分：

- `languages`：用于国际化的语言文件
- `layout`：主题布局模板文件
- `scripts`：hexo脚本插件目录，可以编写一些辅助函数脚本
- `source`：资源文件目录，包括页面样式，js脚本等
- `_config.yml`：主题配置文件

### 局部模板

我们通过分析常见的博客网站可以知道，大部分的博客网站都由三部分组成：顶部导航栏，中间内容区域，以及底部信息展示区域。每次点击导航栏选项跳转页面时，顶部导航栏以及底部信息展示区域是不变的，只是中间的内容区域重新渲染，因此，我们可以将通用的代码抽离成局部模板以复用。

我们在`layout`目录下新建`_partial`目录，在该目录下添加`head.ejs`,`header.ejs`以及`footer.ejs`文件。

`layout/_partial/head.ejs`:

```ejs
<head>
    <meta http-equiv="content-type" content="text/html; charset=utf-8">
    <meta content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=0" name="viewport">
    <title>标题</title>
</head>
```

`layout/_partial/header.ejs`:

```ejs
<header>我是导航栏</header>
```

`layout/_partial/footer.ejs`:

```ejs
<footer>我是底部信息</footer>
```

我们在`layout`中创建`layout.ejs`，并引入`head.ejs`,`header/ejs`和`footer.ejs`文件，`layout.ejs`文件是通用的布局文件模板，我们在后面新增的`ejs`文件都会继承`layout.ejs`，并将其内容填充到`body`中。

`layout/layout.ejs`:

```ejs
<!DOCTYPE html>
<html>
<%- partial('_partial/head') %>
<body>
    <div class="container">
    <%- partial('_partial/header') %>
    <%- body %>
    <%- partial('_partial/footer') %>
    </div>
</body>
</html> 
```

### 首页

首页是我们的网站加载完毕后的第一个页面。

我们在 `layout` 中创建 `index.ejs` 文件，`index.ejs`首页将会继承`layout.ejs`布局模板生成 HTML 文件。

> partial()函数的作用是可以引入其他模板文件，详情参考hexo文档

`layout/index.ejs`:

```ejs
<h1>Hello World</h1>
```

修改**站点配置文件**中的主题配置，使用我们刚刚创建的 `theme-example` 主题：

```yaml
# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: theme-example
```

运行 `hexo server` 开启 Hexo 本地服务器预览，访问 <http://localhost:4000/>。

![](https://ws1.sinaimg.cn/large/c542ee77ly1g0iqkcsz2dj21z20fen08.jpg)

### 编写导航栏和底部信息

前面，我们只是搭了个页面框架，接下来我们就将开始正式开始一步步的完善我们的主题，以下两个文档我们将频繁的使用，最好可以提前阅读一遍有个了解：

- [Hexo | 变量](https://hexo.io/zh-cn/docs/variables)
- [Hexo | 辅助函数](https://hexo.io/zh-cn/docs/helpers)

`layout/_partial/head.ejs`:

```ejs
<head>
    <meta http-equiv="content-type" content="text/html; charset=utf-8">
    <meta content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=0" name="viewport">
    <title><%= config.title %></title>
</head>
```

这里我们使用了`config`全局变量，该变量包含的是站点配置（即站点根目录下 `_config.yml` 中的配置）。除此之外，我还有将经常使用的是`theme`变量，该变量是主题配置（即主题根目录下 `_config.yml` 中的配置），其他变量参见hexo文档。

编辑导航栏部分，`layout/_partial/header.ejs`：

```ejs
<header class="header">
    <div class="title">
        <a href="<%= url_for() %>" class="logo"><%= config.title %></a>
    </div>
    <nav class="navbar">
        <ul class="menu">
            <li class="menu-item">
                <a href="/" class="menu-item-link">Home</a>
            </li>
            <li class="menu-item">
                <a href="/categories" class="menu-item-link">Categories</a>
            </li>
            <li class="menu-item">
                <a href="/tags" class="menu-item-link">Tags</a>
            </li>
            <li class="menu-item">
                <a href="/archives" class="menu-item-link">Archives</a>
            </li>
        </ul>
    </nav>
</header>
```

编辑底部信息部分，`layout/_partial/footer.ejs`：

```ejs
<footer>
    <p>Theme is <a href="/" target="_blank">Theme-example</a> by <a href="<%= config.url %>" target="_blank"><%= config.author %></a></p>
    <p>Powered by <a href="https://hexo.io/" target="_blank" rel="nofollow">hexo</a> &copy; <%- date(Date.now(), 'YYYY') %></p>
</footer>
```

这样，我们就得到了一个包含导航栏和底部信息的简单页面。

![](https://ws1.sinaimg.cn/large/c542ee77ly1g0is6aotexj21z20l2n17.jpg)

### 添加主题配置

实际上我们需要让导航菜单根据我们的需要显示不同的项，上面这种写法不方便修改。所以我们会在主题的配置文件中添加导航菜单的配置。在 `theme-example` 下的配置文件 `_config.yml`，在其中添加需要配置的字段。然后可以通过 `theme` 这个变量来拿到该配置文件中的配置。

`theme-example/_config.yml`:

```yaml
menu:
  home: /
  categories: /categories
  tags: /tags
  archives: /archives
```

这样我们就可以在 `header.ejs` 中使用 `theme.menu` 获取到导航菜单的设置。将 `header.ejs` 修改为：

```ejs
<header class="header">
    <div class="title">
        <a href="<%= url_for() %>" class="logo"><%= config.title %></a>
    </div>
    <nav class="navbar">
        <ul class="menu">
            <% for (name in theme.menu) { %>
            <li class="menu-item">
                <a href="<%- url_for(theme.menu[name]) %>" class="menu-item-link"><%= name %></a>
            </li>
            <% } %>
        </ul>
    </nav>
</header>
```

 这样，我们就可以动态的配置导航的信息了，我们还可以在主题配置文件中添加其他配置项供我们使用。

### 添加文章列表

接着我们完善首页的模板，使其能够显示文章列表。前面已经说过 Hexo 提供了各种有用的变量，在这里将会使用到 `page` 这个变量。`page` 会根据不同的页面拥有不同的属性。具体有什么属性，可以获取到哪些数据可以查看[这里](https://hexo.io/docs/variables.html#Page-Variables)。

那么这里我们会使用 `page` 变量的 `posts` 属性拿到文章数据的集合。编辑 `index.ejs` 文件：

```ejs
<section class="posts">
    <% page.posts.each(function (post) { %>
    <article class="post">
        <div class="post-title">
            <a class="post-title-link" href="<%- url_for(post.path) %>"><%= post.title %></a>
        </div>
        <div class="post-content">
            <%- post.content %>
        </div>
        <div class="post-meta">
            <span class="post-time"><%- date(post.date, "YYYY-MM-DD") %></span>
        </div>
    </article>
    <% }) %>
</section>
```

从 `page.posts` 中获取单篇文章的数据，并获取文章的标题，内容等数据填充到模板中。处理文章创建时间的时候使用了 `date()` 函数，这是 Hexo 提供的时间处理的[辅助函数](https://hexo.io/docs/helpers.html#date)。

由于首页显示文章内容时使用的是 `post.content`，即文章的全部内容。所以首页会显示每一篇文章的内容，实际上我们并不想在首页显示那么多内容，只想显示文章的摘录。

Hexo 提供了 `excerpt` 属性来获取文章的摘录部分，不过这里需要在文章中添加一个 `<!--more-->` 标记。添加了这个标记之后，`post.excerpt` 将会获取到标记之前的内容。如果没有这个标记，那么 `post.excerpt` 会是空的。所以我们可以把首页文章内容部分的 `post.content` 替换成 `post.excerpt`。

```ejs
<div class="post-content">
    <%- post.excerpt %>
</div>
```

### 添加样式

到目前为止，我们完成了首页的整体页面结构，不过由于没添加css样式，因此整体页面非常难看，所以我们需要给页面加上一些样式来美化一下我们的页面。

由于 Hexo 在新建项目的时候会安装 `hexo-renderer-stylus` 这个插件，所以我们无需其他步骤，只需要将样式文件放到 `css` 文件夹中。Hexo 在生成页面的时候会将 `source` 中的所有文件复制到生成的 `public` 文件中，并且在此之前会编译 `styl` 为 `css` 文件。

在 `css` 文件夹中创建 `style.styl`，编写一些基础的样式，并把所有样式 `import` 到这个文件。所以最终编译之后只会有 `style.css` 一个文件。创建 `_partial/header.styl` 与 `_partial/post.styl` 存放页面导航以及文章的样式，并且在 `style.styl` 中 `import` 这两个文件。

`_partial/header.styl`：

```stylus
.header {
    margin-top: 2em
    display: flex
    align-items: baseline
    justify-content: space-between

    .blog-title .logo {
        color: #AAA;
        font-size: 2em;
        font-family: "Comic Sans MS",cursive,LiSu,sans-serif;
        text-decoration: none;
    }

    .menu {
        margin: 0;
        padding: 0;

        .menu-item {
            display: inline-block;
            margin-right: 10px;
        }

        .menu-item-link {
            color: #AAA;
            text-decoration: none;

            &:hover {
                color: #368CCB;
            }
        }
    }
}
```

`index.styl`:

```stylus
.post {
    margin: 1em auto;
    padding: 30px 50px;
    background-color: #fff;
    border: 1px solid #ddd;
    box-shadow: 0 0 2px #ddd;
}

.posts  {
    .post:first-child {
        margin-top: 0;
    }

    .post-title {
        font-size: 1.5em;

        .post-title-link {
            color: #368CCB;
            text-decoration: none;
        }
    }

    .post-content {
        a {
            color: #368CCB;
            text-decoration: none;
        }
    }

    .post-meta {
        color: #BABABA;
    }
}
```

`style.styl`:

```stylus
body {
    background-color: #F2F2F2;
    font-size: 1.25rem;
    line-height: 1.5;
}

.container {
    max-width: 960px;
    margin: 0 auto;
}

@import "_partial/header";
@import "_partial/index";
```

最后，我们需要把样式添加到页面中，这里使用了另外一个辅助函数 [`css()`](https://hexo.io/docs/helpers.html#css):

`layout/_partial/head.ejs`:

```ejs
<head>
    <meta http-equiv="content-type" content="text/html; charset=utf-8">
    <meta content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=0" name="viewport">
    <title><%= config.title %></title>
    <%- css('css/style.styl') %>
</head>
```

至此，我们会看到站点的首页是这个样子的：

![](https://ws1.sinaimg.cn/large/c542ee77ly1g0it4ozg82j21yy0vmq8g.jpg)

### 添加分页

在站点的 `source/_post/` 目录下存放的是我们的文章，现在我们把原本的 `hello-world.md` 复制黏贴 10+ 次，再查看站点首页。会发现，首页只显示了 10 篇文章。

首页显示的文章数量我们可以通过站点配置文件中的 `per_page` 字段来修改，但是我们不可能把所有文章都放在一页，所以我们现在来添加文章列表的分页。

新建 `_partial/paginator.ejs`:

```ejs
<% if (page.total > 1){ %>
    <nav class="page-nav">
    <%- paginator({
        prev_text: "&laquo; Prev",
        next_text: "Next &raquo;"
    }) %>
    </nav>
<% } %>
```

在 `index.ejs` 中添加这个文件的内容：

```ejs
...
</section>
<%- partial('_partial/paginator') %>
```

这里我们使用到了另外的一个辅助函数 [`paginator`](https://hexo.io/docs/helpers.html#paginator)，它能够帮助我们插入分页链接，这里我们只是实现了一个最基本的分页，具体的样式可以自行添加，或者根据文档使用其他配置自定义分页。

![](https://ws1.sinaimg.cn/large/c542ee77ly1g0ity1ov1qj21z014q7a5.jpg)

### 添加文章详情页

文章详情页对应的布局文件是 `post.ejs`，新建 `post.ejs`:

```ejs
<article class="post">
    <div class="post-title">
        <h2 class="title"><%= page.title %></h2>
    </div>
    <div class="post-meta">
        <span class="post-time"><%- date(page.date, "YYYY-MM-DD") %></span>
    </div>
    <div class="post-content">
        <%- page.content %>
    </div>
</article>
```

由于这里是文章的模板，所以变量 `page` 表示的是文章的数据，而不是首页的文章数据集合。

![](https://ws1.sinaimg.cn/large/c542ee77ly1g0ityp6mwmj21z214mtf5.jpg)

### 添加归档页

创建归档页使用的模板文件 `archive.ejs`:

```ejs
<section class="archive">
    <ul class="post-archive">
    <% page.posts.each(function (post) { %>
        <li class="post-item">
            <span class="post-date"><%= date(post.date, "YYYY-MM-DD") %></span>
            <a class="post-title" href="<%- url_for(post.path) %>"><%= post.title %></a>
        </li>
    <% }) %>
    </ul>
</section>
<%- partial('_partial/paginator') %>
```

其实结构跟首页差不多，只是不显示文章内容而已。添加归档页的样式：

`css/_partial/archive.styl`:

```stylus
.archive {
    margin: 1em auto;
    padding: 30px 50px;
    background-color: #fff;
    border: 1px solid #ddd;
    box-shadow: 0 0 2px #ddd;

    .post-archive {
        list-style: none;
        padding: 0;

        .post-item {
            margin: 5px 0;

            .post-date {
                display: inline-block;
                margin-right: 10px;
                color: #BABABA;
            }

            .post-title {
                color: #368CCB;
                text-decoration: none;
            }
        }
    }
}
```

![](https://ws1.sinaimg.cn/large/c542ee77ly1g0iu1jngalj21z214qqax.jpg)

### 添加分类页

分类页跟归档页也类似，相当于根据不同的分类进行归档。

分类页和标签页的模板编写比较特殊，本质上，分类页和标签页属于自定义页面，我们需要新建自定义页面模板`page.ejs`:

```ejs
<% if (is_current(theme.menu.categories)) { %>
<%- partial('_partial/category') %>
<% } else if (is_current(theme.menu.tags)) { %>
<%- partial('_partial/tag') %>
<% } else { %>
<%- partial('_partial/custom') %>
<% } %>
```

这里，我们需要根据当前自定义页面的类型来决定渲染何种自定义页面模板。

新建`_partial/category.ejs`:

```ejs
<section class="archive">
    <ul class="post-archive">
    <% site.categories.each(function (category) { %>
        <span><%= category.name %></span>
        <% category.posts.forEach(function(post) { %>
        <li class="post-item">
            <span class="post-date"><%= date(post.date, "YYYY-MM-DD") %></span>
            <a class="post-title" href="<%- url_for(post.path) %>"><%= post.title %></a>
        </li>
        <% }) %>
    <% }) %>
    </ul>
</section>
```

`site.categories`包括了站点所有的分类信息，可以遍历获取分类信息，其中`category.posts`又包含了该分类的所有文章信息。

需要注意的是，要想在页面中展示分类页，需要先执行`hexo new page categories`生成分类页面，并添加`type`为`categories`：

```markdown
title: categories
date: 2019-02-25 18:19:55
type: "categories"
---
```

后续的标签页类似。

### 添加标签页

标签页与分类页及其类似，只是根据标签进行归档。

新建`_partial/tag.ejs`:

```ejs
<section class="archive">
    <ul class="post-archive">
    <% site.tags.each(function (tag) { %>
        <span><%= tag.name %></span>
        <% tag.posts.forEach(function(post) { %>
        <li class="post-item">
            <span class="post-date"><%= date(post.date, "YYYY-MM-DD") %></span>
            <a class="post-title" href="<%- url_for(post.path) %>"><%= post.title %></a>
        </li>
        <% }) %>
    <% }) %>
    </ul>
</section>
```

### 添加自定义页面

自定义页面与文章详情页类似。

新建`_partial/custom.ejs`:

```ejs
<article class="post">
    <div class="post-title">
        <h2 class="title"><%= page.title %></h2>
    </div>
    <div class="post-meta">
        <span class="post-time"><%- date(page.date, "YYYY-MM-DD") %></span>
    </div>
    <div class="post-content">
        <%- page.content %>
    </div>
</article>
```

我们在主题配置文件中添加自定义页面的菜单：

```yaml
menu:
  home: /
  categories: /categories
  tags: /tags
  archives: /archivesz
  about: /about
```

执行`hexo new page about`进行手动生成页面，编辑文件内容即可。

![](https://ws1.sinaimg.cn/large/c542ee77ly1g0iw9k3aonj21z214safs.jpg)

### Hexo插件

Hexo 有强大的插件系统，让我们能够轻松扩展功能而不用修改核心模块的源码。在 Hexo 中有两种形式的插件：

- 脚本（Scripts）

- 插件（Packages）

如果我们的代码很简单，我们可以编写脚本，只需要把 JavaScript 文件放到 `scripts` 文件夹，在启动时就会自动载入。简单来说，脚本文件可以相当于一些这样的的工具函数，当我们发现Hexo官方提供的函数不能满足我们的需求时，我们可以通过添加一个脚本来实现。

比如，我们现在有这样一个简单的需求，我们想给首页文章列表中的文章块添加一个背景颜色，背景颜色我们可以在文章md文件中定义，如果未定义，则随机选用一种颜色。

首先，我们先在文章md文件中顶部[Front-matter](https://hexo.io/zh-cn/docs/front-matter)添加一个`color`字段：

`_posts/hello-world-1.md`:

```markdown
title: Hello World 1
date: 2019-02-12 17:49:32
categories: 分类1
tags: 
    - 标签1
color: blue
---
```

定义完成后，我们就可以在文章信息字段`post`或者`page`中获取到`color`。

然后，我们需要添加一个脚本函数来根据`color`字段来获取文章块的背景颜色，新增`scripts/getPostBgColor.js`:

```js
const arr = [ 'blue', 'purple', 'green', 'yellow', 'red', 'orange' ];

var getPostBgColor = function(color) {
  if (arr.indexOf(color) >= 0) {
    return `bg-${color}`;
  }
  return 'bg-' + randBgColor();
};

function randBgColor() {
  return arr[randomInt(0, 5)];
}

function randomInt(min, max) {
  return Math.round(Math.random() * (max - min)) + min;
}

hexo.extend.helper.register('getPostBgColor', getPostBgColor);
```

最后一行我们通过`hexo.extend.helper.register`全局注册一个脚本函数，注册完成后，我们就可以在模板文件中和Hexo官方提供的全局函数一样使用。

修改`layout/index.ejs`:

```ejs
...
<article class="post <%= getPostBgColor(post.color) %>">
...
```

添加背景颜色样式，编辑`css/index.styl`:

```stylus
...
.bg-blue {
    background-color: #6fa3ef;
}
.bg-purple {
    background-color: #bc99c4;
}
.bg-green {
    background-color: #46c47c;
}
.bg-yellow {
    background-color: #f9bb3c;
}
.bg-red {
    background-color: #e8583d;
}
.bg-orange {
    background-color: #f68e5f;
}
```

看下效果，Hello World 1这篇文章我们定义了`color：blue`，因此是蓝色，其他文章，我们未定义`color`，因此是随机颜色。

![](https://ws1.sinaimg.cn/large/c542ee77ly1g0jm9hctmlj21z214sq8n.jpg)

这样，我们就实现了随机背景色这个小需求，当然这只是一个非常简单的例子，如果有其他复杂的需求，我们可以通过编写更加复杂的脚本来实现。

### Hexo的数据DB扩展查询

我们已经知道，Hexo已经为我们预先定义了很多常用的变量供我们使用，具体可以在 [Hexo | 变量](https://hexo.io/zh-cn/docs/variables) 查询。但是如果系统提供的变量数据不能满足我们的要求，那我们该怎么办呢？其实我们可以通过扩展查询来获取到我们期望的数据。

其实Hexo所有的文章分类标签等等变量信息，在编译成本地静态文件之前，都是本地存储在一个`db.json`中的，相当于小型的本地数据库，Hexo在运行阶段，所有的数据相关操作其实都是在这个小型数据库上进行操作，其底层使用的查询引擎就是[Warehouse](https://hexojs.github.io/warehouse/Query.html)。因此我们可以通过Warehouse的语法进行自定义扩展查询。

比如我们需要在页面的底部展示全站的最近6篇文章列表，由于Hexo首页只提供了第一页的数据，因此我们可以基于`site`变量进行扩展查询：

```ejs
site.posts.sort({date: -1}).limit(6)
```

`site.posts`表示所有的文章，`sort（{date: -1})`表示按创建时间倒序排列，`limit(6)`表示只取前6条数据，这样我们就可以拿到了全站的最近6文章信息，后续进行相应展示操作即可。

其他更多复杂的扩展查询都可以根据Warehouse语法文档进行按需扩展。

### 总结

其实说白了，Hexo就是把那些 Markdown 文件，按照我们编写的对应布局模板，填上对应的数据生成 HTML 页面，然后在编译的过程中将JS/CSS等文件引入HTML，然后生成每个页面的对应HMTL静态文件。

而Hexo主题的作用就是决定每个布局模板长什么样。

### 参考链接

- [Warehouse 文档](https://hexojs.github.io/warehouse/index.html)
- [Hexo | 主题](https://hexo.io/zh-cn/docs/themes)
- [Hexo | 模板](https://hexo.io/zh-cn/docs/templates)
- [Hexo | 变量](https://hexo.io/zh-cn/docs/variables)
- [Hexo | 辅助函数](https://hexo.io/zh-cn/docs/helpers)
- https://www.ahonn.me/2016/12/15/create-a-hexo-theme-from-scratch/#%E6%B7%BB%E5%8A%A0%E5%BD%92%E6%A1%A3%E9%A1%B5
