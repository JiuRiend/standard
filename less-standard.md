## LESS 规范 <Badge text="1.0.0"/>

本文档的目标是规范组内代码风格，尽可能的使 同事们的 Less 代码风格保持一致，容易被理解和被维护。Less 代码的基本规范和原则与 [Css规范](/code/standard/css-standard.html)  保持一致。

### 代码组织

- #### 代码*必须*（MUST）按如下形式按顺序组织：

  1. #### `@import`

  2. #### 变量声明

  3. #### 样式声明

**示例：**

```less
// ✓
@import "est/all.less";

@default-text-color: #333;

.page {
    width: 960px;
    margin: 0 auto;
}
```

### 变量

- #### Less 的变量值总是以同一作用域下最后一个同名变量为准，务必注意后面的设定会覆盖所有之前的设定。

- #### 变量命名*必须*采用 `@foo-bar` 形式，*不得*使用 `@fooBar` 形式。

- #### 变量命名不得使用 `@font-14` 形式，此种命名方式与直接书写 `font-size: 14px;` 无异，而应该使用 `@font-size-normal` 这种形式。

**示例：**

```less
// ✗
@sidebarWidth: 200px;
@width:800px;

// ✓
@sidebar-width: 200px;
@width: 800px;
```

### 文件导入

- #### 使用 `@import (reference)`  导入 `less` 文件，避免导入的 `less` 文件代码重复打包到最终的 `css` 文件中 

 解释： 

​       原理参见 [Less导入选项reference关键词](https://www.yiibai.com/less/import_options_reference.html)  或自行google

**示例：**

```less
@import (reference) "base.less"
```

