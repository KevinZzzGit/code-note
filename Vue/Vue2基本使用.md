# Vue2基础

## 概述

Vue (发音为 /vjuː/，类似 **view**) 是一款用于构建用户界面的 JavaScript 框架。它基于标准 HTML、CSS 和 JavaScript 构建，并提供了一套声明式的、组件化的编程模型，帮助你高效地开发用户界面。无论是简单还是复杂的界面，Vue 都可以胜任。

##  引入

- CDN

  ```html
  <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
  ```

- 项目搭建

  ```sh
   npm init vue@latest
  ```

## 创建实例并挂载

```js
import Vue from 'vue'
import APP from 'APP.vue'
const app = new Vue({
    /* 根组件实例 */
    render: h => h(App)
}).$mount('#app')
```

## Template部分内容

### 内置组件标签

- ## `<Transition>`

- ## `<TransitionGroup>`

- ## `<KeepAlive>`

- ## `<Teleport>`

- ## `<Suspense>`

### 插值表达式

- 返回组件实例的对象，方法。

  ```vue
  <template>
  	<span>{{ msg }}</span>
  </template>
  ```

- 函数表达式

  ```vue
  <template>
  	<span>{{ 1 + 1 }}</span>
  </template>
  ```

### 指令

- v-html

  渲染HTML

  ```vue
  <template>
  	<span 
            v-html="<span style="color:red">
            v-html directive</span>"
      </span>
  </template>
  ```

- v-model

  数据双向绑定

  ```vue
  <template>
  	<input v-model="count" />
  </template>
  <script>
  export default{
      data() {
          return {
              count: 0
          }
      }
  }
  </script>
  ```

- v-bind(缩写:)

  绑定动态值，用于组件间传递数据。如果绑定的值是 `null` 或者 `undefined`，那么该 attribute 将会从渲染的元素上移除。

  ```vue
  <template>
  	<!-- 绑定一个值  -->
  	<div v-bind:id="dynamicId"></div>
  	<!-- 绑定多个值  -->
  	<div v-bind="objectOfAttrs"></div>
  	<!-- 动态绑定style  -->
  	<div :style="{color:red}"></div>
  	<!-- 动态绑定style  -->
  	<div :class="{ active: isActive }"></div>
  </template>
  <script>
  export default{
      data() {
          return {
              isActive: true,
              objectOfAttrs: {
        			id: 'container',
        			class: 'wrapper'
      		}
          }
      }
  }
  </script>
  ```

- v-for

  用于对元素的重复输出

  ```vue
  <template>
  	<div v-for="(item,index) in 7" :key="index">
          {{ item }}
      </div>
  </template>
  ```

- v-if && v-else && v-else-if

  条件判断，用于控制元素的显隐

  ```vue
  <template>
  	<div v-if="show1">
          show1
      </div>
  	<div v-else-if = "show2">
          show2
      </div>
  	<div v-else>
          no show
      </div>
  </template>
  ```

- v-show

  条件判断，用于控制元素的显隐

  ```vue
  <template>
  	<div v-show="ifShow">
          show
      </div>
  </template>
  ```

- v-on(缩写@)

  事件监听，子组件emit触发，内置参数$event

  ```vue
  <template>
  	<button v-on:click="doFunction"></button>
  	<button @click="doFunction"></button>
  </template>
  ```

- v-bind:is

  配合``<component>``一起使用，导入组件

  ```vue
  <template>
  	<component :bind></component>
  </template>
  ```

- v-slot（缩写#）

  插槽容器，将外部内容放入组件内指定位置。

  ```vue
  <!-- BaseLayout组件 -->
  <template>
  	<section class="base-layout">
           <slot name="default" />
            <slot name="header" />
      </section>
  </template>
  
  <!-- 默认插槽 -->
  <BaseLayout>
  	<template>
      	<!-- 插槽的内容放这里 -->
    	</template>
  </BaseLayout>
  
  <!-- 具名插槽 插入指定name容器中-->
  <BaseLayout>
  	<template v-slot:name>
      	<!-- 插槽的内容放这里 -->
    	</template>
  </BaseLayout>
  
  <!-- 插槽传递scope-->
  <BaseLayout>
  	<template v-slot:name = "myScope">
      	<!-- 插槽的内容放这里,myScope是当前父组件的传入的值 -->
    	</template>
  </BaseLayout>
  
  ```

- v-clock

  用于处理未编译模板闪频情况(展示{{}})，在在组件编译完毕前隐藏原始模板。

- v-once

  仅渲染元素和组件一次，并跳过之后的更新。

- 自定义指令

### 修饰符

内置修饰符用来阻止一些事件的默认行为，可链式调用。

- 事件修饰符
  - (.prevent)
  - (.stop)
  - (.self)
  - (.capture)
  - (.once)
  - (.passive)

-  按键修饰符
  - (.enter)
  - (.page-down)
  - (.tab)
  - (.delete)
  - (.esc)
  - (.space)
  - (.up)
  - (.down)
  - (.left)
  - (.right)
- 鼠标按键修饰符
- 其他修饰符
  - (.lazy)
  - (.number)
  - (.trim)

### 特殊属性

- ref

  用于绑定真实DOM

## Script部分

### Options API

```vue
<script>
import { myMixin } from "./mixin/index";
import Bar from '@/components/Bar/index.vue'
import ComponentA from '@/components/ComponentA/index.vue'
export default {
    // 组件名声明
    name: 'ComponentName',
    // component声明
    components: {
        Bar
    },
    // props声明
    props:{
        name:{
            type:Object,
            required:true,
            default:null,
            // validtor建议在子组件中用计算属性写
            validator:function(value){
                return value !== 'Tom'
            }
        },
      	age:number
    },
    // mixins声明
    mixins:[myMixin],
    // extends
    extends:ComponentA,
    // expose声明 1：用来限定组件暴露的属性
    expose:['publicMethod'],
    // 对象  
    data() {
        return {
            count: 1,
            nameObj:{
                firstName:'',
                secondName:''
            }
        }
    },
    // 计算属性
    computed: {
        doubleCount(){
            return this.count*2
        }
    }
    // 监听属性
    watch: {
    	nameObj:{
    		handler(newVal,oldVal){
                
            }
		},
         deep: true	
	}
    // provide
    provide(){
        // 静态  
        provideValue:this.count
    },
    // inject
    inject:{
        provideValue:{
            default:"null"
        }
    },
    // 方法
    methods: {
        publicMethod(){
          // TODO  
        },
        myEmmiter(){
            this.$emit('myEmit1')
        },
        myFunc() {
           this.count ++ 
        }
    },
    // 生命周期,所有都会绑定this到当前Vue实例
        
    // 会在实例初始化完成、props 解析之后、data() 和 computed 等选项处理之前立即调用。
    beforeCreate(){}, 
    // 在组件实例处理完所有与状态相关的选项后调用
    created(){},
    // 在组件被挂载之前调用。
    beforeMount(){},
    // 在组件被挂载之后调用。
    mounted(){},
    // 在组件即将因为一个响应式状态变更而更新其 DOM 树之前调用。
    beforeUpdate(){},
    // 在组件因为一个响应式状态变更而更新其 DOM 树之后调用。
    updated(){},
    // 在一个组件实例被卸载之前调用。
    beforeDestory(){},
    // 在一个组件实例被卸载之后调用。
    destroy(){},
    // 在捕获了后代组件传递的错误时调用。
    errorCaptured(err,instance,info){},
    // 若组件实例是 <KeepAlive> 缓存树的一部分，当组件被插入到 DOM 中时调用。
    activated(){},
    // 若组件实例是 <KeepAlive> 缓存树的一部分，当组件从 DOM 中被移除时调用。
    deactivated(){},
    // 当组件实例在服务器上被渲染之前要完成的异步函数。返回Promise
    serverPrefetch(){},
        
    // 渲染模板,render函数优先级大于<template>
    render:h=>h('<h1>title<h1>')
}
</script>
```

### Vue实例属性

- emit

  ```vue
  this.$emit('eventName',eventParams)
  ```

- refs

  ```VUE
  this.$ref.formRef
  ```

- attrs

- watch()

- set()

- delete()

- directive()

- use()

- store

- nextTick()

- once()

- observable

  将数据变成响应式数据

- filter

- prototype

## Style部分

  - scoped属性
  - :deep伪类
  - :slotted(div)插槽选择器
  - lang预处理器

