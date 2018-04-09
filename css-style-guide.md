## 基本

### 缩进

一次缩进两个空格，切记不要tab 和 空格混用：

```
.box {
  width: 100px;
}
```

### 大小写

虽然CSS对大小写不敏感，不过那只是针对标签选择器，属性与属性值。也就是说，`SPAN` 和 `span` 是相同的，但 `.box` 与 `.BOX`(`.BOx`, `.Box`) 则完全是两个 class。

因此，为保证代码的一致性，我们规定，在CSS中，不管使用哪一种选择器、不管是属性还是属性值，都必须小写：

```
/* bad */
.BOX {
  Color: #ABCDEF;
}

/* good */
.box {
  color: #abcdef;
}
```

### 空格

为了提高的CSS代码的可读性，需要在代码中加入一些空格。其中，包含以下几部分。

选择器与 `{}` 之间，请在 `{` 前面保留空格：

```
/* bad */
.box{
  width: 100px;
}

.box
{
  width:100px;
}

/* good */
.box {
  width: 100px;
}
```

属性与属性值之间，请在属性 `:` 后面保留空格：

```
/* bad */
.box {
  width:100px;
}

/* good */
.box {
  width: 100px;
}
```

`>`、`+`、`~` 选择器的两边各保留一个空格：

```
/* bad */
.box>p {
  width: 100px;
}

/* good */
.box > p {
  width: 100px;
}
```

如果一个属性存在多个属性值，则必须使用 `,` 结合 空格 一一列出：

```
/* bad */
.box {
  font-family: Arial,sans-serif;
  background: rgba(0,0,0,.5);
}

/* good */
.box {
  font-family: Arial, sans-serif;
  background: rgba(0, 0, 0, .5);
}
```

### 空行（分隔）

`{` 与 属性之间，属性值与 `}` 之间，需要保留一个空行：

```
/* bad */
.box {width: 100px;}

/* good */
.box {
  width: 100px;
}
```

不同选择器应用不同的样式，则不同选择器之间，需要保留一个空行：

```
/* bad */
.box {
  width: 100px;
}
.header {
  height: 100px;
}

/* good */
.box {
  width: 100px;
}

.header {
  height: 100px;
}
```

如果不同选择器，应用同一样式，那么选择器之间也需要保留一个空行：

```
/* bad */
.box, .header {
  width: 100px;
}

/* good */
.box,
.header {
  width: 100px;
}
```

less 中嵌套的选择器，请在前面保留空行：

```
/* bad */
.box {
  width: 100px;
  .box-hd {
    ...
  }
  &::after {
    ...
  }
}

/* good */
.box {
  width: 100px;

  .box-hd {
    ...
  }

  &::after {
    ...
  }
}
```

### 引号

在CSS中，对于一些特定的引用，不能使用单引号，也不允许不使用引号，你必须使用双引号，主要有以下场景。

属性选择器的值必须使用双引号：

```
li[class^="color_"] {
  color: #F00;
}
```

属性的值必须使用双引号：

```
.box {
  font-family: "Microsoft YaHei", sans-serif;
}

.box::after {
  content: "...";
}
```

在很多代码规范中，都建议 `url()` 中不需要使用引号：

```
.box {
  background: url(../img/bg.png);
}
```

但是，如果你使用的是less编译器，如果不加引号，便直接会导致报错。所以，建议给背景加上引号：

```
@imgPath: "../static/img/public";

.box {
  background: url("@{imgPath}/bg.png");
}
```

## 选择器

### 不要为 tag 添加 id 或者 class 限定

由于CSS的读取规则是从右到左的，如果给 tag 限定 id 或者 class，那么，浏览器会先查询 id 或者 class，然后，再从所有的 tag 中去查找符合条件的元素。

但如果不做限定，则直接查找对应的 id 或 class，性能明显高：

```
/* bad */
div.box {
  width: 100px;
}

/* good */
.box {
  width: 100px;
}
```

### 尽可能的使用 class 代替 tag

并非所有的 tag 都定义了 class 属性，但 class 属性必须定义在 tag 上。所以，class 的匹配效率，肯定高于 tag：

```
/* bad */
.box a span {
  color: red;
}

/* good */
.box .link .text {
  color: red;
}
```

### 避免嵌套过深

为了提高性能，也为了减少样式文件的体积，请尽可能保持嵌套层级不超过四级：

```
/* bad */
.box .box-hd .title .slogan {
  color: red;
}

/* good */
.box .box-hd .slogan {
  color: red;
}
```

### 伪元素与伪类

在CSS3之前，伪元素和伪类，这两个概念本身就模糊不清。况且，我们既可以使用单冒号 `:` 来表示伪类，也可以用它来表示伪元素，这也间接上也导致了概念的混淆。

为了加以区分，CSS3决定使用单冒号 `:` 来表示 伪类，使用双冒号 `::` 来表示伪元素。

```
/* 伪类 */
a:hover {}
p:first-child {}

/* 伪元素 */
p::first-line {}
div::before {}
```

但是要注意，因为一些浏览器（IE8以下）不支持双冒号的写法，为了向下兼容老版浏览器，所以，一些伪元素既支持单冒号写法，又支持双冒号写法。比如 `before`、`after`、`first-letter`、`first-line`。


## 属性与值

### 属性单行

为了保证CSS代码的可读性，也为了方便后期维护，禁止将选择器的所有属性写在同一行。

关于CSS文件体积的问题，交给打包工具吧：

```
/* bad */
.box {width: 100px;height: 100px;color: red;}

/* good */
.box {
  width: 100px;
  height: 100px;
  color: red;
}
```

### 属性顺序

在书写CSS属性时，相关属性可以分组，并按建议按照一定的顺序。可参考以下规则：

- 位置 - position, top, right, bottom, left, z-index, float, display, overflow 
- 盒子模型 - margin, padding, border, width, height
- 排版 - font, line-height, letter-spacing, text-align
- 视觉 - color, background, opacity, box-radius(shadow)
- 动画 - transition, animation 

### 属性前缀

为兼容不同浏览器，开发者们不得不写一些私有前缀。建议将前缀由长到短排列，另外，标准（无前缀）属性应该放在最后：

```
/* bad */
.box {
  -webkit-border-radius: 3px;
     -moz-border-radius: 3px;
          border-radius: 3px;

  background: -webkit-linear-gradient(top, #fff 0, #eee 100%);
  background: -moz-linear-gradient(top, #fff 0, #eee 100%);
  background: linear-gradient(to bottom, #fff 0, #eee 100%);
}

/* good */
.box {
  -webkit-border-radius: 3px;
  -moz-border-radius: 3px;
  border-radius: 3px;

  background: -webkit-linear-gradient(top, #fff 0, #eee 100%);
  background:    -moz-linear-gradient(top, #fff 0, #eee 100%);
  background:         linear-gradient(to bottom, #fff 0, #eee 100%);
}
```

### 属性值与简写

CSS中包含很多复合属性（font，background等），通过它们，我们可以使用简写形式来表示多个属性，从而提高编码效率。

```
/* bad */
.box {
  font-weight: bold;
  font-family: sans-serif;
  font-size: 12px;
  line-height: 2;
}

/* good */
.box {
  font: bold 12px/2 sans-serif;
}
```

但也要注意权衡，如果使用简写只表示单个属性，则后面可能会导致其他不必要的样式被覆盖。

```
/* bad */
.box {
  padding: 0 0 0 5px;
}

/* good */
.box {
  padding-left: 5px;
}
```

如果属性值为 -1 ~ 1 之间的小数，可省略 `0`：

```
/* bad */
.box {
  opacity: 0.5;
  background: rgba(0, 0, 0, 0.3);
}

/* good */
.box {
  opacity: .5;
  background: rgba(0, 0, 0, .3);
}
```

如果十六进制的颜色，可以缩写，则应该采用缩写的形式：

```
/* bad */
.box {
  color: #336699;
  background: #fffff;
}

/* good */
.box {
  color: #369;
  background: #fff;
}
```

当属性值为 0 时，可省略相关单位：

```
/* bad */
.box {
  bottom: 0px;
}

/* good */
.box {
  bottom: 0;
}
```

## 避免

### 不要使用 @import

在引入CSS方面，与 `link` 这种方式相比，`@import` 主要有以下问题：

- link 会随页面载入，而 @import 引入的 CSS 要等页面加载完，再进行载入
- @import 是 CSS 提供的，只能在 IE5 上才能识别，而 link 属于 XHTML，兼容性更强
- link 引入的 CSS 权重要高于 @import

### 避免使用expression表达式

根据 高性能网站建设指南 中的描述，expression表达式的求值频率比人们期望的要高。因为他们不只在页面呈现和大小改变时求值，当页面滚动、甚至用户鼠标在页面移动时也会进行求值。

可以考虑使用JavaScript方案来解决相关需求。

### 避免使用通配符 *

在上面说到，因为 tag 选择器查找元素效率比较低，所以，尽量不要使用 tag 选择器。通配符是所有 不同 tag 的集合，更应避免使用。

### 移出空的、没有匹配的样式

移除无匹配的样式，有两个好处：

- 删除无用的样式后可以缩减样式文件的体积，加快资源下载速度
- 对于浏览器而言，所有的样式规则的都会被解析后索引起来，即使是当前页面无匹配的规则。移除无匹配的规则，减少索引项，加快浏览器查找速度


## 其他

### 注释

对于CSS中的注释，请统一使用 `/* xx */` 这种形式，注意在左边 `*` 的后面，以及右边 `*` 的前面，需要保留空格：

```
/* ad */
.ad {
  width: 100px;
}
```

### 图片宽度

网页中的图片经常会被缩放，尤其是在移动端的场景。当提供的图片与显示的图片尺寸不一致时，我们会对图片进行百分比缩放，如果对图片应用 `width: 30%;`（假设30%），则图片可能会显示模糊。并且随着网页的缩放，图片的尺寸可能大于图片实际尺寸，也会出现模糊的情况。

因此，对于图片的缩放，一般不设置 `width`（即默认值 auto）。并且建议使用 `max-width`，一般的值为 `100%`，这样，在页面缩放时，图片最大也只能是它的原始尺寸：

```
.img {
  max-width: 100%;
}
```

### 媒体查询

本着方便维护的原则，尽可能将媒体查询的规则靠近相关选择器，不要把它们放在样式表的最底部，也不要放在独立样式表中：

```
.box {
  width: 1000px;
}

@media (min-width: 480px) {
  .box {
    width: 320px;
  }
}

/* 其他样式 */
...
```

### 定义动画

本着方便维护的原则，尽可能将动画的定义靠近相关选择器，不要把它们放在样式表的最底部，也不要放在独立样式表中：

```
.box {
  animation: fadeIn 1s;
}

@-webkit-keyframes fadeIn {
  100% {
    opacity: 1;
  }
}

@keyframes fadeIn {
  100% {
    opacity: 1;
  }
}
```

### 变换与动画

当使用 `transition` 进行属性变换来处理动画时，为提高性能，则应该指定具体的 `transition-property`：

```
/* bad */
.box {
  transition: all 1s;
}

/* good */
.box {
  transition: width 1s, height 1s;
}
```