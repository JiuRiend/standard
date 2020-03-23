## JS 规范 <Badge text="1.0.0"/>

**本文档的目标是规范组内代码风格，尽可能的使 同事们的 JS 代码风格保持一致，容易被理解和被维护。**

### 代码风格

* #### 使用两个空格 – 进行缩进；

* #### 除需要转义的情况外，字符串统一使用单引号；

* #### 不要定义未使用的变量；

* #### 关键字后面加空格；

* #### 函数声明时括号与函数名间加空格；

* #### 无分号；

* #### 行首不要以  `(` ， `[` ，  or  `   开头，这是省略分号时唯一会造成问题的地方；

* #### 等等……

解释：

1. 本组代码风格应严格按照 [JavaScript Standard Style](https://github.com/standard/standard/blob/master/docs/README-zhcn.md) 规则执行，具体细则参考 [Standard 细则](https://github.com/standard/standard/blob/master/docs/RULES-zhcn.md#javascript-standard-style)。

2. 就目前而言，组内大部分使用 vue-cli 搭建的项目都已经集成好了 Standard 校验，具体配置可以参考根目录中的 .eslintrc.js文件，同时不排除未集成 Standard 校验的项目，此时应该按照 [安装教程](https://github.com/standard/standard/blob/master/docs/README-zhcn.md#install) 并集成到自己的 IDE 中。

3. 当项目中存在第三方库有大量代码无法通过格式校验时，可采用以下方法排除此类文件：

   * 可以在 `package.json` 里添加 `standard.ignore` 属性来配置：

     ```js
     "standard": {
       "ignore": [
         "**/out/",
         "/lib/select2/",
         "/lib/ckeditor/",
         "tmp.js"
       ]
     }
     ```

   * JavaScript Standard 代码规范底层使用的是 [ESLint](http://eslint.org/)。所以如果需要隐藏某些警告，使用  `/* eslint-disable */`  即可     

### 命名规则

**基本原则：全英文命名，且命名规则应遵循基本的见名思意原则**

::: tip 见名思意 原则

在兼顾长度和可读性的情况下应尽可能准确的表达出其作用

:::

* #### 变量名 和 函数名 使用小驼峰命名，即首字母小写，每个有意义的单词首字母大写

* #### 常量 使用 全部字母大写，单词间下划线分隔 的命名方式

* #### `boolean` 类型的变量使用 `is` 或  `has` 开头

**示例：**

```js
var loadingModules = {}
const HTML_ENTITY = {}
function stringFormat (source) {}
var isReady = false
var hasMoreCommands = false
```

### 注释

* #### 单行注释，`//` 后跟一个空格；若另起一行，缩进与下一行被注释说明的代码一致

* #### 多行注释使用 `/**  **/` 的方式

**示例：**

```js
// 单行注释
 
/**
 * 这是一个多行注释
 * 多行注释的第二行
 */
```

* #### 方法/函数注释，应对方法功能和注意事项做简要描述，且注明所需参数的类型和说明

* #### 对 Object 中各项的描述， 必须使用 `@param` 标识。

**示例：**

```js
/**
 * 函数描述
 *
 * @param {string} p1 参数1的说明
 * @param {string} p2 参数2的说明，比较长
 *     那就换行了.
 * @param {number=} p3 参数3的说明（可选）
 * @return {Object} 返回值描述
 */
function foo(p1, p2, p3) {
    var p3 = p3 || 10;
    return {
        p1: p1,
        p2: p2,
        p3: p3
    }
}
 
 
/**
 * 函数描述
 *
 * @param {Object} option 参数描述
 * @param {string} option.url option项描述
 * @param {string=} option.method option项描述，可选参数
 */
function foo(option) {
    // TODO
}
```

### `var` 、`let` 、`const`

* #### 在块级作用域中，使用 `let` 代替 ` var` 使用

* #### 在无特殊要求的` for` 循环中，强制要求使用 `let` 定义索引

* #### 在定义常量时，使用 `const`

### 类型检查

#### 类型检测优先使用 `typeof`。对象类型检测使用 `instanceof`。`null` 或 `undefined` 的检测使用 `== null`

**示例：**

```js
// string
typeof variable === 'string'
 
// number
typeof variable === 'number'
 
// boolean
typeof variable === 'boolean'
 
// Function
typeof variable === 'function'
 
// Object
typeof variable === 'object'
 
// RegExp
variable instanceof RegExp
 
// Array
variable instanceof Array
 
// null
variable === null
 
// null or undefined
variable == null
 
// undefined
typeof variable === 'undefined'
```

### 类型转换

* #### 转换成 `string` 时，使用 `+ ''`

* #### 转换成 `number` 时，通常使用 `+ ` 或者  `- 0` 

* #### `string` 转换成 `number`，要转换的字符串结尾包含非数字并期望忽略时，使用 `parseInt`

* #### 使用 `parseInt` 时，必须指定进制

* #### 转换成 `boolean` 时，使用 `!!`

* #### `number` 去除小数点，使用 `Math.floor` / `Math.round` / `Math.ceil`，不使用 `parseInt`

**示例：**

```js
// good
num + ''
 
// bad
new String(num)
num.toString()
String(num)
 
_______________________
// good
+str;
str - 0
// bad
Number(str)
 
________________________
var width = '200px'
parseInt(width, 10)
 
________________________
// good
parseInt(str, 10)
 
// bad
parseInt(str)
 
 
________________________
var num = 3.14
!!num
 
________________________
// good
var num = 3.14
Math.ceil(num)
 
// bad
var num = 3.14
parseInt(num, 10)
```

### 对象

* #### 使用对象字面量 `{}` 创建新 `Object`

* #### 对象创建时，如果一个对象的所有 `属性` 均可以不添加引号，建议所有 `属性` 不添加引号

* #### 对象创建时，如果任何一个 `属性` 需要添加引号，则所有 `属性` 建议添加 `'`

* #### `属性` 访问时，尽量使用  `.` 

* #### `for in` 遍历对象时, 使用 `hasOwnProperty` 过滤掉原型中的属性

**示例：**

```js
// good
var obj = {}
 
// bad
var obj = new Object()
 
________________________
var info = {
    name: 'someone',
    age: 28
}
 
________________________
info.age
info['more-info']
 
________________________
var newInfo = {}
for (var key in info) {
    if (info.hasOwnProperty(key)) {
        newInfo[key] = info[key]
    }
}
```

### 数组

* #### 使用数组字面量 `[]` 创建新数组，除非想要创建的是指定长度的数组

* #### 遍历数组不使用 `for in`

**示例：**

```js
// good
var arr = []
 
// bad
var arr = new Array()
 
________________________
var arr = ['a', 'b', 'c'];
 
// 这里仅作演示, 实际中应使用 Object 类型
arr.other = 'other things'
 
// 正确的遍历方式
for (let i = 0, len = arr.length; i < len; i++) {
    console.log(i)
}
 
// 错误的遍历方式
for (var i in arr) {
    console.log(i)
}
```

