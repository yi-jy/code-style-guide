## 网页头部

### 文档声明

为保障页面正常渲染，需要在页面顶部加入文档类型，此标签告知浏览器的解析器，以哪种文档类型规范(HTML、XHTML、HTML5)来解析这个文档。其中，`html` 需要小写：

```html
<!DOCTYPE html>
```

### lang属性

规范表明，应该在网页html标签加 `lang` 属性。这样，可以给语音工具 和 翻译工具提供帮助，告诉它们应当如何去对网页进行发音和翻译。

```html
<!DOCTYPE html>
<html lang="zh-cn">
    ...
</html>
```

推荐阅读 [语言列表](https://www.sitepoint.com/iso-2-letter-language-codes/) 以及 [lang="zh" 或者 "zh-cn" 的相关讨论](https://www.zhihu.com/question/20797118) 。


### 字符编码

在HTML中指定字符集，有助于浏览器轻松、快速并以正确的形式对HTML代码进行解析。

```html
<head>
  <meta charset="UTF-8">
  ...
</head>
```

如果未指定字符编码集，则浏览器一般都会在执行脚本和渲染页面前，把字节流缓存，之后再搜索可进行解析的字符集，或以默认的字符集来解析页面代码，这会导致消耗不必要的时间。

### 渲染模式

由于文档解析存在怪异模式和标准模式，而且你不知用户的浏览器所处模式。对于一些怪异模式的浏览器，可以强制使用渲染模式：

```html
<head>
  <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
  ...
</head>
```

### 移动端友好

如果希望你的网页在移动端也能正常展示，则需要对 `viewport` 进行设置：

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0, user-scalable=no" />
```

### 文件引入

根据HTML5规范, 我们在网页中引入 CSS 和 JS 文件时，不需要显示的指定 `type` 属性，因为 `text/css` 和 `text/javascript` 分别是他们的默认值。所以，推荐不加 `type` 属性的形式：

#### CSS 

```html
<!-- 外部 CSS -->
<link rel="stylesheet" href="page.css">

<!-- 内部 CSS -->
<style>
...
</style>
```

#### JAVASCRIPT

```html
<!-- 外部 JAVASCRIPT -->
<script src="page.js"></script>

<!-- 内部 JAVASCRIPT -->
<script>
...
</script>
```

推荐阅读 [link](https://www.w3.org/TR/2011/WD-html5-20110525/semantics.html#the-link-element)、[style](https://www.w3.org/TR/2011/WD-html5-20110525/semantics.html#the-style-element)、[script](https://www.w3.org/TR/2011/WD-html5-20110525/scripting-1.html#the-script-element) 的使用。

另外，如果有性能要求，可以考虑对外部的JS文件使用 `defer` 和 `async` 属性。

还有，如果位于页面底部的JS文件，它的位置必须位于 `</body>` 前面：

```html
<!-- bad -->
...
<footer>...</footer>
</body>
<script src="page.js"></script>
</html>

<!-- bad -->
...
<footer>...</footer>
</body>
</html>
<script src="page.js"></script>

<!-- good -->
...
<footer>...</footer>
<script src="page.js"></script>
</body>
</html>
```


## 基本

### 代码缩进

关于使用空格还是tab，一直饱受争议。实际上，无论使用哪一种缩进方式，都没有对错。重要的是，公司内部约定，达到团队统一协作，建议使用两个空格（这样与内嵌JavaScript也统一）。

```html
<div class="box">
  <p>text</p>
</div>
```

### 大小写

HTML内容需要统一使用小写形式：

```html
<!-- bad -->
<A HREF="/">xx.com</A>

<!-- good -->
<a href="">xx.com</a>
```

### 标签关闭

在HTML5中，短标签允许不关闭，但其他标签必须得关闭：

```html
<!-- good -->
<img src="flower.png">

<!-- bad -->
<span>text

<!-- good -->
<span>text</span>
```

### 标签嵌套

标签的使用，必须符合嵌套规则，各标签之间不允许随意嵌套：

```html
<!-- bad -->
<p>
  <div>text</div>
</p>

<!-- good -->
<div>
  <p>text</p>
</div>
```

可遵循以下规则：

- 一般来说，内联元素只能嵌套内联元素（`a` 元素除外）
- 块级元素不能被 `p` 元素嵌套
- `tbody` 只能被 `table` 嵌套
- `dt` `dd` 只能被 `dl` 嵌套
- `li` 必须嵌套在 `ul`，`ol` 中

推荐阅读 [HTML5与ＨTML4的元素嵌套规则](https://www.jianshu.com/p/5fc78e64a415)

### 属性使用双引号

元素的属性值必须使用双引号，不运行使用单引号、或者不使用引号：

```html
<!-- bad -->
<a href='/'><a>

<a href=/></a>

<!-- good -->
<a href="/">link</a>
```

### boolean属性无需声明值

对于标签内一些布尔属性，XHTML需要每个属性声明取值，但在HTML5中，则不需要明确指定对应的布尔值：

```html
<!-- bad -->
<input type="text" disable="disabled">

<!-- good -->
<input type="text" disabled>

<input type="checkbox" value="1" checked>
```

### 自定义属性

如果用户需要在HTML标签上，自定义一些属性。建议以 `xxx-` 为前缀，推荐使用 `data-`：

```html
<!-- bad -->
<div type="a"></div>

<!-- good -->
<div data-type="a"></div>
```

### 属性顺序

通常标签上挂载着很多属性，为了确保代码的可读性，可遵循如下规则：

- class
- id, name
- data-*
- src, for, type, href, value
- title, alt
- role, aria-*

### id、class 命名规则

在同一个页面中，请确保 `id` 的唯一性。另外，`id` 和 `class` 的命名请采用小写字母结合 `-` 分隔的形式：

```html
<!-- bad -->
<div class="CONTENT_BOX">text</div>
<div class="CONTENT-BOX">text</div>

<!-- good -->
<div class="content-box">text</div>
```

### 语义化

所谓语义化，就是可以让机器更容易理解你的网页内容。这样，既符合搜索引擎对网页的检索机制，同时，在css文件未加载或者丢失时，又可以清楚看到页面层级结构。

常见的XHTML语义标签：

- p - 段落
- h1 ~ h6 - 层级标题
- strong,em - 强调
- ins - 插入，del - 删除，abbr - 缩写，code - 代码标识，cite - 引述，q - 引用
- blockquote - 长篇引用
- ul - 无序列表，ol - 有序列表
- dl,dt,dd - 定义列表

HTML5更是新增了一些标签：

- header - 头部，footer - 底部
- nav - 导航，
- article - 文档，section - 内容块
- aside - 侧边栏，广告、导航条（区分主体内容）
- hgroup - 分组
- figure & figcaption - 元素的组合
- mark - 突出显示内容，time - 日期或时间，address - 联系信息


## 图片

### 禁止给 img 设定一个空的地址

比如，我们创建了一个图片，并且暂时设置图片的地址为空，希望在未来动态的去修改它。但是即使图片的地址为空，不同浏览器依旧会以默认的规则去请求空地址：

- IE会把img的空地址解析为当前页面地址的目录地址，并重新请求
- 早些版本的Webkit内核浏览器 与Firefox 会把空地址解析为当前页面的地址

所以，不要指定空的地址，这也是HTML性能要求的准则之一。

```html
<!-- bad -->
<img src="">

<!-- good -->
<img src="flower.png">
```

### 避免给所有的 img 添加 title 属性

title 属性给 img 提供了额外信息。

但并不是所有的 img 都需要指定 title 的，因为给图片设置此属性，会增加网页体积。而且，鼠标经过时，会有文字覆盖，间接的影响图片预览体验。

如果存在一张图说明不了内容本身（一朵花，什么花呢？），又或者一个没有文字，只有图标的链接（什么链接？）的场景。那么，建议加上 title 属性：

```html
<img src="flower.png" title="牡丹">
```

还有，需要说明的是，title 属性并不是 img 独有的。

### 给图片加 alt 属性

alt 属性是 img 的替代文字。

当图片加载失败、或者用户浏览器被禁用、又或者遭遇视觉障碍以及屏幕阅读器用户时，alt 内容可以给浏览器提供有关该 img 的文字说明。另外，alt 属性也是 img SEO的重要依据。所以，给所有图片都加上 alt 属性吧：

```html
<img src="flower" alt="鲜花">
```


## 表单

### 控件关联

文本控件必须关联 `label` 标签，可通过以下两种形式来关联：

- 将控件置于 label 内
- label 的 for 属性指向控件的 id

据说IE6不支持第一种嵌套的形式，你可以考虑结合这两种形式。

```html
<label><input name="confirm" type="checkbox">我同意</label>

<label for="username">用户名：</label> <input id="username" name="username" type="text">

<label for="confirm"><input id="confirm" name="confirm" type="checkbox">我同意</label>
```

在应用了 `label` 后，当点击文本内容时，关联的控件也会被选中。

### 按钮类型

默认情况下，`button` 元素的type属性值 为 `submit`，如果表单中的 `button` 未指定类型，则点击按钮会导致表单提交。

所以，针对表单中的不同按钮，需要显式的指定按钮类型：

```html
<button type="button">按钮</button>
<button type="reset">重置</button>
<button type="submit">提交</button>
```

### 原生提交

通常情况下，我们都是先使用JavaScript处理完相关逻辑，再进行提交。但如果用户浏览器禁用了JavaScript，为保证数据能正常提交，则需要平稳处理：

```html
<form action="/login" method="post">
  <input name="username" type="text" placeholder="用户名">
  <input name="password" type="password" placeholder="密码">
  <button type="submit">提交</button>
</form>
```


## 其他

### 命名

#### 页面结构

- 页面 page > page-content
- 头部 header
- 内容 content
- 内容主体 main
- 侧边栏 aside
- 尾部 footer
- 栏目 column
- 外围 wrapper

#### 导航 & 侧边栏

- 导航 nav
- 顶部导航 top-nav
- 子导航 sub-nav
- 顶部栏 nav-bar
- 底部栏 tool-bar
- 菜单 menu
- 子菜单 sub-menu
- 标题 title
- 摘要 summary

#### 功能

- 模板 tpl-xx
- 标志 logo
- 广告 banner
- 登录 login login-in(out) 
- 注册 regsiter
- 搜索 search
- 轮播 slide
- 按钮 btn
- 图标 icon
- 标签页 tab
- 列表 list
- 单项 item
- 表单 form
- 弹层 popop
- 滚动 scroll
- 进度 progress
- 步骤 step
- 服务 service
- 指南 guide
- 提示 tips
- 备注 note
- 消息 msg
- 上传 upload
- 下载 download

#### 状态

- 状态 status
- 选中 selected
- 当前 current
- 活跃 active
- 禁用 disabled

### iframe

尽可能不使用 `iframe` 元素，因为：

- `iframe` 会阻塞主页面的Onload事件
- `iframe` 和主页面共享连接池，而浏览器对相同域的连接有限制，所以会影响页面的并行加载

可使用 组件化 或者 jQuery 的 `$.load` 的方法来加载网页片段。

如果非要使用 `iframe`，则可以通过动态创建 `iframe`添加src属性值，来规避上面的问题。

### table

尽可能不使用 `table` 元素，主要出于如下考虑：

- 没有任何语义性
- 相同界面，`table` 要比其它HTML标记占更多的字节
- css 对于table的样式处理，并非特别友好
- 浏览器要解析到 `</table>` 才会完全显示，加载速度慢，性能差。其它HTML不会有此问题

### HTML5降级处理

对于一些低版本的浏览器，可能会出现不支持的元素或者特性。此时，你需要做好平稳降级：

```html
<canvas>
  你的浏览器不支持canvas！
</canvas>
```

### 多媒体播放源

由于各个浏览器对多媒体播放源的支持度不同，为保证多媒体在不同浏览器正常播放，你需要为同一资源，提供多个文件格式：

```html
<video controls>
  <source src="foo.mp4" type="video/mpeg">
  <source src="foo.ogg" type="video/ogg">
</video>
```