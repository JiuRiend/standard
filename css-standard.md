## CSS 规范 <Badge text="1.0.0"/>

本文档的目标是规范组内代码风格，尽可能的使 同事们的 CSS 代码风格保持一致，容易被理解和被维护。
虽然本文档是针对 CSS 设计的，但是在使用各种 CSS 的预编译器(如 less、sass、stylus 等)时，适用的部分也应尽量遵循本文档的约定。


### 空格、缩进、属性简写
使用 2 个空格做为一个缩进层级，不允许使用 4 个空格 或 tab 字符。
选择器 与 `{` 之间必须包含空格。
属性名 与之后的 `:` 之间不允许包含空格， `:` 与 属性值 之间必须包含空格。
列表型属性值 书写在单行时, 后必须跟一个空格。
属性能简写时，必须简写，方便阅读和维护。
属性值为 0 时，建议不带单位，若带单位，须保持单个项目整体统一！
属性定义必须另起一行。
示例：
```css
/* good */
.selector {
  margin: 5px 10px;
  padding: 0 10px 5px 10px;
 font-family: Arial, sans-serif;
}
```

### 选择器
当一个 rule 包含多个 selector 时，每个选择器声明必须独占一行。
示例：
```css
/* good */
.post,
.page,
.comment {
  line-height: 1.5;
}

/* bad */
.post, .page, .comment {
  line-height: 1.5;
}
/* 建议 >、+、~ 选择器的两边各保留一个空格。若不保留空格，须保持单个项目整体统一！
示例：*/
/* good */
main > nav {
    padding: 10px;
}

label + input {
    margin-left: 5px;
}

input:checked ~ button {
    background-color: #69C;
}
/* 属性选择器中的值必须用双引号包围。
示例：*/
/* good */
article[character="juliet"] {
    voice-family: "Vivien Leigh", victoria, female;
}

/* bad */
article[character='juliet'] {
    voice-family: "Vivien Leigh", victoria, female;
}
```

### 清除浮动
当元素需要撑起高度以包含内部的浮动元素时，通过对父级的伪类设置 clear 进行 clearfix。
示例：
```css
.clearfix::after {
    content: "";
    display: block;
    clear: both;
}
.clearfix{
    *zoom: 1;
}
```

### 颜色
RGB 颜色值必须使用十六进制记号形式 `#rrggbb` 。不允许使用 rgb() ，带有 alpha 的颜色信息可以使用 rgba() 。使用 rgba() 时每个逗号后必须保留一个空格。
示例：
```css
/* good */
.success {
    box-shadow: 0 0 2px rgba(0, 128, 0, .3);
    border-color: #008000;
}

/* bad */
.success {
    box-shadow: 0 0 2px rgba(0,128,0,.3);
    border-color: rgb(0, 128, 0);
}
/*
颜色值不允许使用命名色值。
颜色值中的英文字符采用小写。如不用小写也需要保证同一项目内保持大小写一致。
示例：
*/
/* good */
.success {
    background-color: #aaccaa;
    color: #90ee90;
}

/* good */
.success {
    background-color: #AACCAA;
    color: #90EE90;
}

/* bad */
.success {
    background-color: #ACA;
    color: #90ee90;
}
```

### 字体相关
`font-family` 属性中的字体族名称应使用字体的英文 Family Name，其中如有空格，须放置在引号中。
解释：所谓英文 Family Name，为字体文件的一个元数据，常见名称如下：

| 字体            | 操作系统 | Family Name         |
| --------------- | -------- | ------------------- |
| 宋体 (中易宋体) | Windows  | SimSun              |
| 黑体 (中易黑体) | Windows  | SimHei              |
| 微软雅黑        | Windows  | Microsoft YaHei     |
| 微软正黑        | Windows  | Microsoft JhengHei  |
| 华文黑体        | Mac/iOS  | STHeiti             |
| 冬青黑体        | Mac/iOS  | Hiragino Sans GB    |
| 文泉驿正黑      | Linux    | WenQuanYi Zen Hei   |
| 文泉驿微米黑    | Linux    | WenQuanYi Micro Hei |

示例：

```css
h1 {
    font-family: "Microsoft YaHei";
}
/*
font-family 按「英文字体在前、中文字体在后」、「效果佳 (质量高/更能满足需求) 的字体在前、效果一般的字体在后」的顺序编写，最后必须指定一个通用字体族( serif / sans-serif )。
解释：     
*/
/* Display according to platform */
.article {
    font-family: Arial, sans-serif;
}

/* Specific for most platforms */
h1 {
    font-family: "Helvetica Neue", Arial, "Hiragino Sans GB", "WenQuanYi Micro Hei", "Microsoft YaHei", sans-serif;
}
/*
需要在 Windows 平台显示的中文内容，其字号应不小于 12px，建议14px为默认大小。
line-height 在定义文本段落时，应使用数值，且应定义默认行高1.25
*/
.container {
    line-height: 1.25;
}
```

更详细说明可参考 [本文](http://www.zhihu.com/question/19911793/answer/13329819) 。

### 过渡与动画
尽可能在浏览器能高效实现的属性上添加过渡和动画。能使用 `transform` 实现的效果绝不使用 `position`
解释：
见本文，在可能的情况下应选择这样四种变换：

```css
transform: translate(npx, npx);
transform: scale(n);
transform: rotate(ndeg);
opacity: 0..1;
/*
典型的，可以使用 translate 来代替 left 作为动画属性。示例：
*/
/* good */
.box {
    transition: transform 1s;
}
.box:hover {
    transform: translate(20px); /* move right for 20px */
}

/* bad */
.box {
    left: 0;
    transition: left 1s;
}
.box:hover {
    left: 20px; /* move right for 20px */
}
```



