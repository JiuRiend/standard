## Vue 规范<Badge text="1.0.0"/>

### 组件名
命名须遵循以下原则：

有意义的名词、简短、具有可读性，避免与 HTML 标签冲突，结构清晰。
以大写开头，采用大驼峰的命名方式
文件夹命名主要以功能模块代表命名 同时还需要注意：必须符合自定义元素规范


示例：
```js
// 反例
export default {
    name: 'articleItem'
}

// 正例
export default {
    name: 'ArticleItem'
}
```
### Prop
定义 Prop 的时候应该始终以驼峰格式（camelCase）命名，且定义的时候应该尽量详细的指定其类型，默认值以及验证。

示例：
```js
// 反例
props: ['attrM', 'attrA', 'attrZ']

// 正例
props: {
    attrM: Number,
    attrA: {
        type: String,
        required: true
    },
    attrZ: {
        type: Object,
        // 数组/对象的默认值应该由一个工厂函数返回
        default: function ( ) {
            return {
                msg: '成就你我'
            }
        }
    },
    attrE: {
        type: String,
        validator: function (v) {
            return !(['success', 'fail'].indexOf(v) === -1) 
        }
    }
}
```

### v-for
在执行 `v-for` 遍历的时候，总是应该带上 key 值使更新 DOM 时渲染效率更高。

示例：
```vue
// 反例
<ul>
    <li v-for="item in list">
        {{ item.title }}
    </li>
</ul>

// 正例
<ul>
    <li v-for="item in list" :key="item.id">
        {{ item.title }}
    </li>
</ul>
```

### v-if / v-else-if / v-else
若同一组 `v-if` 逻辑控制中的元素逻辑相同，Vue 为了更高效的元素切换，会复用相同的部分，例如： value 。为了避免复用带来的不合理效果，应该在同种元素上加上 key 做标识。 
```vue
示例：
// 反例
<div v-if="hasData">
    <span>{{ mazeyData }}</span>
</div>
<div v-else>
    <span>无数据</span>
</div>

// 正例
<div v-if="hasData" key="mazey-data">
    <span>{{ mazeyData }}</span>
</div>
<div v-else key="mazey-none">
    <span>无数据</span>
</div>
```

### 指令缩写
为了统一规范始终使用指令缩写，使用 `v-bind` ， `v-on` 并没有什么不好，这里仅为了统一规范。

示例：
```vue
<input :value="mazeyUser" @click="verifyUser">
```

### 样式

为了避免样式冲突，整个项目建议全都使用 scoped 特性，若遇到使用动态生成 DOM 节点无法赋上 scopeId 的而无法修改样式的情况，应另行新建 reset 样式表全局引入，做单独的样式重置，

### 总结
本文档旨在规范公链应用组前端编码规范，力求达到任何人阅读他人代码都能快速理解并着手开发，其他未总结到的编码格式规范应严格按照 ESLint 格式要求编写即可。