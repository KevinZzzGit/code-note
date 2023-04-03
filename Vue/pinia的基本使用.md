# Pinia的基本使用

## 概述

Pinia 是 Vue.js 团队成员专门为 Vue 开发的一个全新的状态管理库

## 核心概念

### State

### Getters

### Actions

### 使用案例

```js
import { defineStore } from 'pinia'
export const userStore = defineStore('user', {
    state: () => {
        return { 
            count: 1,
            arr: []
        }
    },
    getters: { 
    	computedCount(state){
            return state.count;
        },
        computedCount2(){
            return this.count + 1
        }
    },
    actions: { 
    	changeCount(count:number){
            this.count = count
        }
    }
})
```

```vue
<script lang="ts">
    import { storeToRefs } from 'pinia'
	import { userStore } from '../store'
    // 访问数据 解构对于基本类型会复制副本，需要用ToRefs转响应式
	const { count } = storeToRefs(userStore())
    
 
    // 更新数据
    count++ ;
    userStore.$patch({count:2})
    userStore.changeCount(2)
</script>

```



## 对比VueX

- 更好的Ts支持
- 取消冗余的Mutation
- actions支持同步和异步
- 取消module，扁平化管理。自动注册到实例上。
