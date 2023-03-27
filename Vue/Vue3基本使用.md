# Vue3的基本使用

## 概述

## 引入

## 创建实例

```js
import { createApp } from 'vue'
import App from './App.vue'

const app = createApp(App)
app.mount('#app')
```

## Script部分

### CompositionAPI

```vue
<script lang="ts">
import { myMixin } from "./mixin/index";
import { defineComponent, defineProps, provide, inject, nextTick , h } from 'vue'
import { ref, reactive, toRefs } from 'vue'
import { computed, watch} from 'vue'
//  setup 的执行时间是在原来的beforeCreate和created之间,所以取消了这两个生命周期函数
import { 
onbeforeMount, onMounted,
onBeforeUpdate, onUpadted,
onBeforeUnmount, onUnmounted,
onActivated, onDeactivated,
onErrorCaptured, 
onRenderTracked, onRenderTriggered,
onServerPrefetch } from "vue"

import Bar from '@/components/Bar/index.vue'
    
// 将功能模块封装在一个函数中
function useMousePosition(){
    const position = reactive({x:0,y:0})
    const update = e => {
        position.x = e.pageX
        position.y = e.pageY
    }
    
    onMounted(()=>{
        window.addEventListener("mousemove",upadate)
    })
    onUnmounted(()=>{
        window.removeEventListener("mousemove",upadate)
    })
    // 通过toRefs,传入Proxy对象，将普通对象的复制，解构后的值变成响应式对象
    return toRefs(position)
}
export default defineComponent({
    // 组件名声明
    name: "ComponentName",
    // components声明
    components: {
        Bar
    },
    // props声明
    props:{
        propsName:{
            type:Object,
            required:true,
            default:null,
            // validtor建议在子组件中用计算属性写
            validator:function(value){
                return value !== 'Tom'
            }
        },
      	age:number,
        inputValue:string
    },
    // mixins声明
    mixins:[myMixin],
	// extends
    extends:ComponentA, 
    // expose声明 1：用来限定组件暴露的属性
    expose:['publicMethod']
    // emits 定义能够上报的事件
    emits:['myEmitter']
    
    // setup 的执行时间是在原来的beforeCreate和created之间
    // 第一个参数 props
    // 第二个参数 context --> { artrs, emit, slots }
    setup(props,context) {
    	// 声明响应式数据
    	const name = ref("KEVIN")
        const message = ref("message")
        // 声明复杂响应式数据
        const obj = reactive({x:0,y:0})
        // compositionAPI 模块写法优势
        const position = useMousePosition()
        
        // provide 
        provide(MessageSymbol, message);
        // inject
        const injectMessage = inject(ProvideMessageSymbol);
        
        // 使用props
        console.log(props.propsName)
        // 使用data,调用value属性
        console.log(name.value)
        
        // 计算属性
        const uppercaseMessage = computed(() =>message.value.toUpperCase());
        
        // 监听属性
        watch(
      		() => props.inputValue,
      		(newValue, oldValue) => {
        		console.log(`inputValue changed from ${oldValue} to ${newValue}`);
      		},
            deep:true,
            immediate:true
    	);
        
        // 方法
        const myFunc = () => {}
        
         // 在组件被挂载之前调用。
        onbeforeMount(()=>{}),
        // 在组件被挂载之后调用。
        onMounted(){},
        // 在组件即将因为一个响应式状态变更而更新其 DOM 树之前调用。
        onBeforeUpdate(){},
        // 在组件因为一个响应式状态变更而更新其 DOM 树之后调用。
        onUpdated(){},
        // 在一个组件实例被卸载之前调用。相当于Destroy
        onBeforeUnmount(){},
        // 在一个组件实例被卸载之后调用。相当于Destroy
        onUnmounted(() => {
            await nextTick(()=>{
            // 访问计算属性和方法代替this方法
        		console.log('Uppercase message:', context.refs.uppercaseMessage.textContent);
        		context.emit('message-updated', message.value);
            })
        },
        // 在捕获了后代组件传递的错误时调用。
        onErrorCaptured(err,instance,info){},
        // 若组件实例是 <KeepAlive> 缓存树的一部分，当组件被插入到 DOM 中时调用。
        onActivated(){},
        // 若组件实例是 <KeepAlive> 缓存树的一部分，当组件从 DOM 中被移除时调用。
        onDeactivated(){},
        // 当组件实例在服务器上被渲染之前要完成的异步函数。返回Promise
        onServerPrefetch(){},		
        // 注册一个调试钩子，当组件渲染过程中追踪到响应式依赖时调用。这个钩子仅在开发模式下可用。
        onRenderTracked(()=>{})
        // 注册一个调试钩子，当响应式依赖的变更触发了组件渲染时调用。这个钩子仅在开发模式下可用。
    	renderTracked(()=>{})
    	
    
        // defineComponent 需要在此暴露需要的方法和data
        return {
            name,
            message,
            obj,
            position,
            myFunc
        }
	},
     render(){
         h('h1', this.message),
      	 h('h2', this.uppercaseMessage),
      	 h('button', { onClick: this.increment }, `Increment count: ${this.count}`)
     }
})
</script>
```

### ``setup``语法糖

```vue
<script lang="ts">
exports default {
    // 例如name、props等属性仍可以在此定义，如需在setup语法糖中定义，需要引入函数
}
</script>
<script lang="ts" setup>
    import BaseComponent from './BaseComponent.js';
    import ChildComponent from './ChildComponent.vue';
	import { ref } from 'vue'
    import { defineProp, defineExpose, withDefaults, defineComponent }
    // 在setup语法糖中，无需 defineComponent 函数，因为组件会自动定义,变量会默认导出,不需要return
    const message = ref("hello")
    
    // 定义components
    
    
    // 定义extends
    const MyComponent = defineComponent({
        components:{
            ChildComponent
        },
  		extends: BaseComponent,
	});
    // 使用extends
    MyComponent.methods.logBaseMessage.call(MyComponent);
    
    // 定义props
    const props = withDefaults(defineProps({
        customProp:"Default"
    }))
    // 定义emits
     const emits = defineEmits(['parentClick','parentChang'])
   	// 定义mixins
    const mixin = BaseMixin.setup()
    // 定义expose
    defineExpose({
  		...mixin
	});
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

- 删除set()

- 删除delete

- directive()

- use()

- store

- nextTick()

- once()

- reactive

  原observable用法

- 删除filter

- prototype

- app.config.globalProperties

  替换prototype使用

## Style部分

  - scoped属性
  - :deep伪类
  - :slotted(div)插槽选择器
  - lang预处理器

