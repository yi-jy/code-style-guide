## 基本

### 缩进层级

为了保证在各大IDE中显示的代码格式一致，请使用两个空格：

```js
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

```js
// bad
let str = "string content";

// good
let str = 'string content';
```

### 单行长度

若一行代码的长度太长，有的编辑器可以设置为自动换行，但有的编辑器可能会出现横向滚动条。为了能更方便在编辑器预览代码，建议单行长度不要超过80个字符。

### 换行

上面说到单行长度最好不要超过80个字符，如果超过，则我们需要手动换行。比如，一个很长的判断：

```js
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

```js
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

```js
// bad
str = 'string content';

// good
var str = 'string content';
```

若使用ES6编码，则建议声明变量使用 `let`，而声明常量则使用 `const`：

```js
// bad
var str = 'string content';
var domain = 'xx.com';

// good
let str = 'string content';
const DOMAIN = 'xx.com';
```

声明变量时，`=` 两边应该保留空格。并且，在声明多个变量时，`,` 应该放在变量值后：

```js
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

```js
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

```js
// bad
var arr = new Array();

// good
var arr = [];
```

数组的每项后面的 `,` 需保留空格：

```js
// bad
var arr = ['str',1,true];

// good
var arr = ['str', 1, true];
```

数组的循环，建议使用 `for` 或者 `for of`：

```js
var arr = [2, 4, 6];

for(var i = 0; i < arr.length; i++) {
  console.log(i, arr[i]);
}

for(item of arr) {
  console.log(item);
}
```

由于数组是引用类型，当直接赋值拷贝数组时，可能存在相互影响的问题。建议使用 `slice` 来处理拷贝问题（也可以通过先 `join` 再 `split` 的方式）：

```js
// bad
var arr1 = [1, 2, 3];
var arr2 = arr1;

// good
var arr1 = [1, 2, 3];
var arr2 = arr1.slice();
```

若需要将伪数组转换为数组，也可通过 `slice` 方法：

```js
function foo() {
  var args = Array.prototype.slice.call(arguments);
  
  ...
}
```

### 对象

使用对象字面量，而非构造函数的形式来创建对象：

```js
// bad
var obj = new Object();

// good
var obj = {};
```

和JSON不同，对象的属性名无需加引号。并且，属性值后面的 `:` 与属性值直接需保留一个空格，另外，如果对象存在多个属性时，需每个属性独占一行：

```js
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

如果对象中包含了多个方法，为了代码的可读性，可以考虑在方法前保留空行：

```
var util = {
  version: '1.0',

  addClass: function() {

  },

  removeClass: function() {
    
  }
};
```

访问对象单个属性时，为防止属性名无法正常读取，最好是通过中括号 `[]` 来进行访问：

```js
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

```js
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

```js
// 函数声明
function foo() {

}

// 函数表达式
var foo = function() {
  
}
```

其中，在函数声明里，需要注意以下几点：

- `function` 与函数名之间需保留一个空格
- 函数名与 `(` 之间无需空格
- `()` 与 `{` 直接也需要保留一个空格
- 函数里包含多个参数，则每个参数之间需要用 `, ` 来分隔

函数表达式里类似。

```js
// bad
function foo(a,b,c) {

}

foo(1,2,3);

// good
function foo(a, b, c) {

}

foo(1, 2, 3);
```


### 代码块

在判断和循环的语句块中，需要在关键词的两边保留空格：

```js
// bad
if(a == 1){

}

while(true){

}

switch(value){

}

for(var i = 0; i < 10; i += 1){

}

// good
if (a === 1) {

}

while (true) {

}

switch (value) {

}

for (var i = 0; i < 10; i += 1) {

}
```

在代码块的前面，建议保留一个空行：

```js
// bad
var a = 1;
for (var i = 0; i < 10; i += 1) {

}

// good
var a = 1;

for (var i = 0; i < 10; i += 1) {

}
```

在 `if` 判断中，为防止JavaScript弱类型转换，必须使用严格类型符号 `===` 来进行比较：

```js
// bad
if (a == 1) {

}

// good
if (a === 1) {

}
```

一些判断可通过三元运算符来简写，但是如果条件与结果过长，则必须采用换行的形式：

```js
// bad
var tips = isMember ? aVeryVeryVeryLongStringA : aVeryVeryVeryLongStringB;

// good
var tips = isMember 
  ? aVeryVeryVeryLongStringA
  : aVeryVeryVeryLongStringB;
```

另外，如果存在多重三元运算符，则需要注意层级缩进：

```js
function joinAct(isMember) {

  return isMember
    ? memberLevel > 3
      ? sendSomething()
      : alertMemberLevelIsLower()
    : alertNotIsMember()
}
```


## 注释

在JavaScript中，有多种注释方案，我们可以根据不同场景使用不同方案：

### 单行注释

一般而言，单行注释采用双斜线，即 `//`。`//` 与底部的代码必须保持相同的缩进，并且 `//` 与注释说明之间必须保留一个空格：

```js
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

```js
// bad
foo(); // some comment

// good
// some comment
foo();
```

另外，在单行注释的前面，必须保留空一行：

```js
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

```js
// bad
function foo() {

  // 解释的第一行
  // 解释的第二行
}
```

遇到此类情况，你该考虑使用多行注释。多行注释以 `/**` 开头，`*/` 结尾：

```js
// good
function foo() {

  /**
   * 解释的第一行
   * 解释的第二行
   */
}
```

和单行注释一样，`*` 与注释说明之间须留有一个空格，多行注释之前也需要保留空行，多行注释与底部代码对齐。以下都为不好的做法：

```js
// bad
function foo() {

  /**
   *这段注释的问题是
   *符号（*）后面没有空格
   */
}

function foo() {
  /**
   * 这段注释的问题是
   * 前面没有空行
   */
}

function foo() {

/**
 * 这段注释的问题是
 * 没有对底部代码对齐
 */
  const a = 1;
}
```


## 避免

### 全局变量

全局变量容易出现命名冲突、难以调试、访问性能等问题。因此，在日常编码中，尽量避免全局变量。以下两种变量的定义，都会导致全局变量：

```js
// bad a 没有使用 var 定义
a = 1;

// bad c 为全局变量
function foo() {
  var b = c = 2;
}
```

### with

通过改变包含上下文解析变量的方式，`with` 可以轻松的设置和访问对象的属性，但它也会降低性能，通过with包裹的代码块，作用域链将会额外增加一层，降低索引效率。

更重要的是，它让一些代码看起来很迷惑：

```js
var cat = {
  color: 'black',
  age: 3
}

var color = 'red';

with (cat) {
  color = 'brown'
}
```

如果不运行代码，你很难知道 `with` 中 `color` 的重新赋值，是针对全局变量 `color`，还是它的 `color` 属性（实际上，`cat` 有对应的属性，则是对它属性的重新赋值。否者，则对全局变量赋值）。

### eval

由于 `eval` 会混淆语义，使用起来容易出错，最主要的是它自身是一个编译器，能执行一切传入它的内容，如果传入它的内容为不信任的源，这让程序不能保证预期。比如：

```js
var a = 1;

eval('a = 2');

eval('alert("warning")');
```

所以，应该尽量避免使用 `eval` 函数。与 `eval` 有着类似功能的，还有 `Function`、 `setTimeout()` 以及 `setInterval()`。

### ++ --

递增和递减运算符本意是让代码写起来更方便，但同时，它也会使代码看上去难以理解，尤其是 `++` 和 `--` 位于变量之前。为了代码的易读性，推荐使用 `+=` 和 `-=`：

```js
// bad
var a = 1;

a++;

// good
var a = 1;

a += 1;
```


## 代码组织与优化

### 命名

虽然命名一直是编码中的一个难题，但良好的命名，可大大提高代码的可读性。JavaScript中的命名统一采用驼峰式命名法，根据语法功能，可分为变量（常量）和函数：

#### 变量 & 常量的命名

变量 & 常量的名称应该结合实际场景。其中，要注意以下几点：

- 变量以小写字母开头，之后每个单词首字母大写
 - 布尔值类型的变量，应该使用 `is`, `has` 或者 `can` 开头
 - 正则类型的变量，建议使用 `RegExp` 结尾 
- 常量名应该全部大写，并且用 `_` 连接

另外，由于系统原因，`Android` 在变量名中应该首字符大写，而 `iOS` 则是首字母小写，后面两个字母大写。

```js
// bad
let MyName = 'xx';
let getCount = 10;
let domain_name = 'xx.com';

// good
let catColor = 'black';
let isFound = false;
let AndroidVersion = '2.0.0';
let iOSVersion = '2.0.0';
let emptyRegExp = /^\s*$/;
const DOMAIN_NAME = 'xx.com';
```

数组对象命名类同。

#### 函数的命名

普通函数名也采用驼峰命名法，即首字母小写，后面每个单词的首字母大写。并且建议第一个单词为 `动词`，后续单词应尽量结合具体场景：

| **动词** | **解释** |
| ----- | ----- |
| is   | 返回布尔值，表示是否处于某种状态 |
| has  | 返回布尔值，判断是否含有某个值 |
| can  | 返回布尔值，判断是否能执行某个动作 |
| set  | 进行某种操作 |
| get  | 返回一个数据，获取内容 |

一些好的命名：

```js
// bad
function myName() {
  
}

// good
function getUserData() {

}

function checkForm() {

}

function isDone() {

}
```

另外，构造函数和类的名称，首字符应该大写：

```js
function Person(name) {
  this.name = name;
}

class Person {
  constructor(name) {
    this.name = name;
  }
}
```

### 优化

#### 全局变量

为了避免全局变量带来的影响，可采用自执行的匿名函数来包裹代码：

```js
;(function(env) {
  var version = '1.0';

  function addClass() {
    
  }
})(window);
```

前面的 `;` 是为了防止打包多个文件时，该文件被识别为函数调用。或者，采用命名空间：

```js
var util = {
  version: '1.0',

  addClass: function() {

  }
};
```

#### 值的判断

判断布尔值变量时，可直接取反判断：

```js
// bad
if (isLoading === false) {

}

// good
if (!isLoading) {

}
```

判断变量值时，可使用 `||` 来代替：

```js
// bad
function getVal(val) {

  if (val) {
    return val;
  } else {
    return 'some text';
  }
}

// good
function getData(val) {
  return val || 'some text'; 
}
```

判断数组里是否含有项时，可直接判断：

```js
// bad
if (listData.length > 0) {

}

// good
if (listData.length) {

}
```

#### 类型转换

转字符串时，建议使用 `+ ''`：

```js
var str = num + '';
```

转数字时，建议使用 `+`：

```js
var num = +str;
```

转布尔值时，建议使用 `!!`：

```js
var hasMoreData = !!dataList.length;
```

#### 字符串拼接

动态生成字符串，可通过数组的 `join`，而非连接符：

```js
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

#### DOM操作

对于一些重复的操作，可集中处理：

```js
// bad
$('.box .content1').show();
$('.box .content2').show();

// good
$('.box .content1, .box .content2').show();
```

### 函数

#### 函数拆分

通常情况下，一个函数的行数应该控制在50行以内。主要考虑到如果将所有的代码都塞入一个函数，一来需要翻屏才能看完整个函数，这影响可读性。二来函数内部代码量大、逻辑繁多，导致难以维护和复用。

所以，有必要对函数里的代码进行拆分。

```js
// bad
function login() {
  var userName = form.get('userName');
  var password = form.get('password');

  if (!userName) {
    return alert('未输入用户名')；
  }

  if (!password) {
    return alert('未输入密码')；
  }

  $.ajax({
    data: {
      userName: userName,
      password: password
    },
    success: function(res) {

      if (res.statusCode === 200) {
        $('.login-in').hide();
        $('.login-out').show();

        $('.user-name').html(res.userName);
      } else {
        alert('登录失败');
      }
    }
  });
}
```

上面代码中，将表单验证、ajax数据请求、dom操作都混在一个函数里。可读性差，无法复用。可这样改进：

```js
// good
function login() {
  checkForm(function(userName, password) {

    $.ajax({
      data: {
        userName: userName,
        password: password
      },
      success: function(res) {

        if (res.statusCode === 200) {
          setUserStatus(res);
        } else {
          alert('登录失败');
        }
      }
    });
    
  });
}

function checkForm(callback) {
  var userName = form.get('userName');
  var password = form.get('password');

  if (!userName) {
    return alert('未输入用户名')；
  }

  if (!password) {
    return alert('未输入密码')；
  }

  typeof callback === 'function' && callback(userName, password);
}

function setUserStatus(res) {
  $('.login-in').hide();
  $('.login-out').show();

  $('.user-name').html(res.userName);
}
```

#### 参数设计

将默认参数放在后面，这样，在函数调用时，可以不传默认参数：

```js
// bad
function foo(opts = {}, name) {

}

// good
function foo(name, opts = {}) {

}
```

如果函数的参数比较多的话，说明函数处理的逻辑也比较多，此时，你可以考虑拆分你的函数。

如果参数不确定个数或者传入的顺序，可考虑使用对象来作为参数的数据类型，将原有的参数挂在对象属性上：

```js
function foo(opts = {}) {

}

foo({
  name: 'xx',
  age: 30,
  callback: function() {}
})
```

请求数据的参数建议使用 `data`，返回数据的参数建议使用 `res` ：

```js
function getUserInfo(data) {
  $.ajax({
    data: data,
    success: function(res) {
      console.log(res);
    }
  })
}
```