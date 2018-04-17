## 基本

### 缩进层级

为了保证在各大IDE中显示的代码格式一致，请使用两个空格：

```
// bad 
function add(a, b) {
    return a + b;
}

// good
function add(a, b) {
  return a + b;
}
```

### 引号

由于html标签的属性值，都是双引号 `""`。为了明确区分，也为了拼接字符串时单引号和双引号之间的转换。因此，建议在JavaScript中，使用单引号 `''` 来包含字符串：

```
// bad
let str = "string content";

// good
let str = 'string content';
```

### 单行长度

若一行代码的长度太长，有的编辑器可以设置为自动换行，但有的编辑器可能会出现横向滚动条。为了能更方便在编辑器预览代码，建议单行长度不要超过80个字符。

### 换行

上面说到单行长度最好不要超过80个字符，如果超过，则我们需要手动换行。比如，一个很长的判断：

```
// bad
if (form.get('username') && form.get('age') && form.get('phone').length === 11 && form.get('address')) {

}

// good
if (form.get('username') &&
  form.get('age') &&
  form.get('phone').length === 11 &&
  form.get('address')) {

}
```

又比如，一个很长的字符串拼接：

```
// bad
let html = '<div class="box"><h3 class="box-title"></h3><p>some content</p></div>';

// good 保持缩进的字符串拼接
let html = '' +
  '<div class="box">' +
    '<h3 class="box-title"></h3>' +
    '<p>some content</p>' +
  '</div>';

// good 数组拼接
let html = [
  '<div class="box">',
    '<h3 class="box-title"></h3>',
    '<p>some content</p>',
  '</div>'
].join('');
```

### 空行

在代码中适当的插入空行，可让代码易读性大大提高，一般而言，建议在以下几种情况下的前面保留一个空行：

- 函数（方法）之间
- 函数局部变量与第一条语句之间
- 单行和多行注释的的前面
- 逻辑片段、代码块之间
- 在文件的末尾保持一个空行


## 语法

### 变量 & 常量

为避免全局变量的污染，必须通过 `var` 来声明一个变量：

```
// bad
str = 'string content';

// good
var str = 'string content';
```

若使用ES6编码，则建议声明变量使用 `let`，而声明常量则使用 `const`：

```
// bad
var str = 'string content';
var domain = 'xx.com';

// good
let str = 'string content';
const domain = 'xx.com';
```

声明变量时，`=` 两边应该保留空格。并且，在声明多个变量时，`,` 应该放在变量值后：

```
// bad
var str='string content';

// bad
var str1 = 'string content'
  , str2 = 'string content';

// bad str2 会变全局变量
var str1 = str2 = 'string content';

// good
var str1 = 'string content',
  str2 = 'string content';

// good 强烈建议，字符多的问题交给压缩工具
var str1 = 'string content';
var str2 = 'string content';
```

在作用域的顶部声明变量，并建议后面再赋值的变量位于最后：

```
// bad
function foo() {
  
  var something;

  ...

  var str = 'string content';

  ...
}

// good
function foo() {
  var str = 'string content';
  var something;

  ...
}
```

### 数组

使用数组字面量，而非构造函数的形式来创建数组：

```
// bad
var arr = new Array();

// good
var arr = [];
```

数组的每项后面的 `,` 需保留空格：

```
// bad
var arr = ['str',1,true];

// good
var arr = ['str', 1, true];
```

数组的循环，建议使用 `for` 或者 `for of`：

```
var arr = [2, 4, 6];

for(var i = 0; i < arr.length; i++) {
  console.log(i, arr[i]);
}

for(item of arr) {
  console.log(item);
}
```

由于数组是引用类型，当直接赋值拷贝数组时，可能存在相互影响的问题。建议使用 `slice` 来处理拷贝问题（也可以通过先 `join` 再 `split` 的方式）：

```
// bad
var arr1 = [1, 2, 3];
var arr2 = arr1;

// good
var arr1 = [1, 2, 3];
var arr2 = arr1.slice();
```

若需要将伪数组转换为数组，也可通过 `slice` 方法：

```
function foo() {
  var args = Array.prototype.slice.call(arguments);
  
  ...
}
```

### 对象

使用对象字面量，而非构造函数的形式来创建对象：

```
// bad
var obj = new Object();

// good
var obj = {};
```

和JSON不同，对象的属性名无需加引号。并且，属性值后面的 `:` 与属性值直接需保留一个空格，另外，如果对象存在多个属性时，需每个属性独占一行：

```
// bad
var obj = {
  'name': 'xx',
  'age': 8
};

var obj = {
  name:'xx',
  age:8
};

var obj = {name: 'xx', age: 8};

// good
var obj = {
  name: 'xx',
  age: 8
};
```

访问对象单个属性时，为防止属性名无法正常读取，最好是通过中括号 `[]` 来进行访问：

```
var obj = {
  'my-name': 'xx',
  age: 8
};

// bad 错误
console.log(obj.my-name);

// good
console.log(obj['my-name']);
``` 

但若要访问对象的所有属性，可通过 `for in` 结合 `hasOwnProperty` 来处理：

```
var obj = {
  name: 'xx',
  age: 8
};

for (var key in obj) {
  if (obj.hasOwnProperty(key)) {
    console.log(key, obj[key]);
  }
}
```

### 函数

函数一般有两种定义形式，即 `函数声明` 和 `函数表达式`：

```
// 函数声明
function foo() {

}

// 函数表达式
var foo = function() {
  
}
```

其中，在函数声明里，`function` 与函数名之间需保留一个空格，`()` 与 `{` 直接也需要保留一个空格，函数表达式里类似。如果函数里包含参数，则每个参数之间需要用 `, ` 分隔：

```
// bad
function foo(a,b,c) {

}

foo(1,2,3);

// good
function foo(a, b, c) {

}

foo(1, 2, 3);
```


### 判断

if else

while

switch

三目

### 循环

for

for in

for of

### 其他

动态生成字符串，可通过数组的 `join`，而非连接符：

```
// bad
function getHtml(data) {
  var html = '<ul>';

  for (var i = 0; i < data.length; i++) {
    items += '<li>' + data[i].text + '</li>';
  }

  html += '</ul>'

  return html;
} 

// good
function getHtml(data) {
  var items = [];

  for (var i = 0; i < data.length; i++) {
    items[i] = '<li>' + data[i].text + '</li>';
  }

  return '<ul>' + items.join('') + '</ul>';
}
```

## 注释

在JavaScript中，有多种注释方案，我们可以根据不同场景使用不同方案：

### 单行注释

一般而言，单行注释采用双斜线，即 `//`。`//` 与底部的代码必须保持相同的缩进，并且 `//` 与注释说明之间必须保留一个空格：

```
// bad
//some comment
foo();

//some comment
  foo();

// good
// some comment
foo();
```

既然是单行注释，所以，请保证每行注释单独占一行：

```
// bad
foo(); // some comment

// good
// some comment
foo();
```

另外，在单行注释的前面，必须保留空一行：

```
// bad
function add(a, b) {
  // b 的默认值为 1
  var b = b || 1;

  return a + b;
}

// good
function add(a, b) {

  // b 的默认值为 1
  var b = b || 1;

  return a + b;
}
```

### 多行注释

如果注释内容过多，你不能以连续多行的单行注释的这种形式来对内容进行注释，比如：

```
// bad
function foo() {

  // 解释的第一行
  // 解释的第二行
}
```

遇到此类情况，你该考虑使用多行注释。多行注释以 `/**` 开头，`*/` 结尾：

```
// good
function foo() {

  /**
   * 解释的第一行
   * 解释的第二行
   */
}
```

和单行注释一样，`*` 与注释说明之间须留有一个空格，多行注释之前也需要保留空行，多行注释与底部代码对齐。以下都为不好的做法：

```
// bad
function foo() {

  /**
   *解释的第一行
   *解释的第二行
   */
}

function foo() {
  /**
   * 解释的第一行
   * 解释的第二行
   */
}

function foo() {

/**
 * 解释的第一行
 * 解释的第二行
 */
  const a = 1;
}
```


## 避免

with 语句

eval 语句

