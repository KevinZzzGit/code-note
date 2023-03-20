# VueX的基本使用

## 概述

Vuex 是一个专为 Vue.js 应用程序开发的**状态管理模式 + 库**。它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。

## 引入

- CDN

  ```vue
  <script src="/path/to/vuex.js"></script>
  ```

- 项目引入

  ```js
  import { createStore } from 'vuex'
  const store = createStore({
    // 设置严格模式
    strict: true,
    state () {
      return {
        count: 0
      }
    },
    mutations: {
      increment (state) {
        state.count++
      }
    }
  })
  const app = createApp({ /* 根组件 */ })
  
  // 将 store 实例作为插件安装
  app.use(store)
  ```

## 核心概念

### State

### Getter

### Mutation

### Action

### Module

``namespace``

​	命名空间

``root``

​	绑定属性到全局命名空间上

### 使用案例

```js
const moduleA = {
  state: () => ({ ... }),
  mutations: { ... },
  actions: { ... },
  getters: { ... }
}

const moduleB = {
  state: () => ({ ... }),
  mutations: { ... },
  actions: { ... }
}


const store = createStore({
  state () {
    return {
      count: 0
    }
  },
  getters:{
      doubleCount(state,getters，rootState,rootGetters) {
          return state.count*2
      }
  },
  mutations: {
    increment (state,payload) {
      state.count++
    }
  },
  // 提交异步操作，提交mutations
  actions: {
      increment(context){
          context.commit('increment')
      }
  },
  modules: {
    a: moduleA,
    b: moduleB
  }
})
```

## Store实例

- ``dispatch``

  分发Action

  ```js
  store.dispatch('actionName',{arg1:arg1Val})
  ```

- ``commit``

  提交mutation

  ```js
  store.commit('mutationName',{arg1:arg1Val})
  ```