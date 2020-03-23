## 魔法数字问题 <Badge text="1.0.0"/>

**本文旨在找出一种解决前端项目开发中在业务代码中存在对大量魔法数字问题，提高代码可读性以及可维护性**

* #### `魔法数字` 在业务代码中，表示一定意义的数字或字符串；

### 场景

1. 后端返回数据中所有非直接可读的数据，如合约状态state，合约类型type，合约生成方式等。

2. 前端针对某种业务逻辑定义的标示性变量且在多处使用

```js
export default{
  data () {
    type: '0' // 0-类型1，1-类型2，2-类型3...
  },
  methods: {
    check (params) {
      if (params === '0') {
        //...
      } else if (params === '1') {
        //...
      }
    }
}
```

### 后果

1. 这样做虽然业务逻辑并没有错，但是当他人或者一段时间之后自己阅读代码时，需要花大量时间去捋代码逻辑才能理解代码中的`0`或`1`所代表的是什么意思，不利于阅读和维护。
2. 当需求涉及到需要增加数据类型的数量时，前端需要将所有用到该变量判断到地方都修改一遍，产生额外都工作量，并且嗨很容易漏掉某个地方。

### 解决方案

* 针对单独的`js`插件以及具有封闭作用域的方法类，应该在作用域顶部提前定义好变量和常量，在业务逻辑中使用常量做实际比较即可。

```js
function check (state) {
  const SUCEESS = '200'
  const FAIL = '400'
  if (state === SUCEESS) {
    // ...
  } else if (state === FAIL) {
    //...
  }
}
```

* 针对每个业务模块提取出一个`codeConfig.js`文件，用于枚举模块中各业务所需的具体变量对应关系，并讲此枚举对象暴露到全局，其数据结构如下。

```js
// codeConfig.js
export default {
  HTTP_SUCCESS_CODE: 200, // 请求成功
  CONTRACT_STATUS: { // 合约状态
    UN_SUBMIT: {
      CODE: '0',
      MSG: '未提交'
    },
    UNDER_VERIFY: {
      CODE: '1',
      MSG: '审核中'
    },
    VERIFY_FAIL: {
      CODE: '2',
      MSG: '审核驳回'
    },
    VERIFY_PASS: {
      CODE: '3',
      MSG: '审核通过'
    },
    RUNNING: {
      CODE: '4',
      MSG: '运行中'
    },
    STOP: {
      CODE: '5',
      MSG: '终止'
    }
  },
  CONTRACT_TYPE: { // 合约类型
    KNOWLEDGE_CONTRACT: {
      CODE: '0',
      MSG: '知识合约'
    },
    ALGORITHMIC_CONTRACT: {
      CODE: '1',
      MSG: '算法合约'
    }
  },
  CONTRACT_CREATE_MODE: { // 合约生成方式
    FROM_DRAG: {
      CODE: '1',
      MSG: '可视化设置'
    },
    FROM_UPLOAD: {
      CODE: '0',
      MSG: '本地上传'
    }
  }
}
```
再在main.js中把`codeConfig`注册到全局？

```js
// main.js

import codeConfig from '@/SM_Modules/utils/codeConfig'
import filters from './utils/filter'

// 注册全局过滤器
Object.keys(filters).forEach(key => {
  Vue.filter(key, filters[key])
})

// 可根据业务模块具体划分
Vue.prototype.$codeConfig = codeConfig

new Vue({
  el: '#app',
  render: h => h(App)
})

```

在工具类函数或全局过滤器中使用时

**全局过滤器的提取原则应该遵守：在两个以上的组件中使用到同一个过滤器时，应提取到全局，记住目的是为了减少重复代码，和减少需求改变时需要修改到地方即可。**

```js
import codeConfig from './codeConfig'

export default {
  // 根据状态渲染不同字段
  contractStatusFilter(code) {
    let result = ''
    for (let key in codeConfig.CONTRACT_STATUS) {
      if ((code + '') === codeConfig.CONTRACT_STATUS[key].CODE) {
        result = codeConfig.CONTRACT_STATUS[key].MSG
      }
    }
    return result
  },
  // 根据状态返回不同到class名组合，当然还需书写对应class的样式
  contractStatusClassFilter(code) {
    let result = 'contract-status-icon '
    for (let key in codeConfig.CONTRACT_CREATE_MODE) {
      if ((code + '') === codeConfig.CONTRACT_CREATE_MODE[key].CODE) {
        result += key
      }
    }
    return result
  }
}
```

像这样注册到全局到好处在于，在具体业务组件中不用再去关心是否还需引用`codeConfig`，而可以直接通过全局`this`访问，并可以在`html`中直接书写判断逻辑。

```js
// component.vue
<template>
  <div v-if="contractType === $codeConfig.CONTRACT_TYPE.KNOWLEDGE_CONTRACT.CODE">
    <!--直接访问-->
    <div class="{{ contractState | contractStatusClassFilter }}">{{ contractState | contractStatusFilter }}</div>
  </div>
</template>

<script>
import contractApi from '@/api/contract'
export default {
  name: 'component',
  data() {
    return {
      contractType: '',
      contractState: ''
    }
  },
  mounted() {
    this.getInitData()
  },
  methods: {
    getInitData() {
      contractApi.getInitData().then(res => {
        if (res.code === this.$codeConfig.SUCCESS_CODE) { // 直接通过this访问
          this.contractType = res.result.type
          this.contractState = res.result.state
          // ...
        }
      })
    }
  }
}
</script>

```

### 总结

代码是写给别人看的，只是附带能在机器上运行。高质量的代码应该是在追求较少的代码量的同时保证他人阅读后能快速理解并作手开发，针对现在的政务通项目情况，`codeConfig`的引入方式还有待商榷，所以目前这个解决方案也仅作抛砖引玉之用。
