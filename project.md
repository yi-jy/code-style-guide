## 文件目录

一个项目的根目录一般至少包含两个目录、两个文件，即：

- src 项目源文件
- dist 编译打包后的文件
- package.json 项目信息以及相关安装依赖
- README.md 项目说明

`dist` 是根据具体的项目结合打包工具的配置生成，这里只对 `src` 目录进行说明：

### HTML

页面模板一般存放在根目录下的 `views`，比如 `views/index.html`

### 图片

图片文件存放在 `images` 文件夹，比如 `images/xx.png`

### 字体

字体文件存放在 `font` 文件夹，比如 `font/xx.ttf`

为了兼容各大浏览器，文件夹中包含的字体格式应该有 `.eot`、`ttf`、`woff` 这么三种格式。

### CSS

css文件存放在 `css` 文件夹，比如：`css/page.css`。如果开发使用了预处理器 less，则less文件存放在 `less` 文件夹，比如  `less/page.less`

### JAVASCRIPT

js文件存放在 `js` 文件夹，比如：`js/page.js`。如果是通用的工具库，可考虑多增加一层 `libs` 目录，比如 `js/libs/jquery.min.js`


一个项目的完整目录参考如下：

```
project
  -- package.json
  -- README.md
  -- dist
  -- src
	-- views
	  -- index.html
    -- static
      -- images
        -- logo.png
      -- less
        -- base.less
        -- page.less
      -- fonts
        -- xx.ttf
      -- javascripts
        -- libs
          -- jquery.min.js
        -- page.js
```


## 文件命名

一般情况下，文件名都应该以小写字母来命名，并且使用中划线连接。采用小写有以下好处：

- 跨平台性好，可移植性强  主要考虑多 Linux 系统（服务器）大小写敏感
- 易读  大写字母往往比小写的难读
- 易区分 一些系统和配置文件都是大写，自定义的项目文件采用小写似乎更好区分

### 项目名

项目名以中划线连接，比如 `project-name`

### 目录名

目录名以中划线连接，比如 `project-app`

### HTML

html页面名称以中划线连接，比如 `news-detail.html`

### 图片

图片文件名以中划线连接，比如 `icon-home.png`

### CSS 

css文件名以中划线连接，比如 `news-detail.css` 或者 `news-detail.less`

### JAVASCRIPT

JavaScript文件名以中划线连接，比如 `page-slider.js`
 