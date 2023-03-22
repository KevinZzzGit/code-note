# Vue2中的组件通信

## 基本场景

- 父子组件
- 兄弟组件
- 祖孙组件
- 非关系组件

## 通信方案

1. props + emit

   - 适用于 父子组件、兄弟组件、祖孙组件

   ```vue
   // parentCom.vue
   <template>
   	<childCom :name = "name" @onNameChange="onNameChange"></childCom>
   </template>
   <script>
   	export default {
           data(){
               return {
                   name:'Kevin'
               }
           }
           methods:{
               onNameChange(newVal){
                   this.name = newVal
               }
           }
       }
   </script>
   
   // ChildCom.vue
   <template>
   	<span>{{name}}</span> // Kevin
   	<button @click = "$emit('onNameChange','newVal')"> </button>
   </template>
   <script>
       export default {
           props:['name'],
       }
   </script>
   ```

2. $attrs + $listeners

   - $attrs 以对象形式返回所有不是props的属性(`class`和 `style`除外)

   ```vue
   // grandParentCom.vue
   <template>
   	<parentCom name="grandParentName" @updateName="onUpdateName"></parentCom>
   </template>
   <script>
   </script>
   
   // parentCom.vue
   <template>
   	<childCom v-bind="$attrs" v-on="$listeners"></childCom>
   </template>
   <script>
       export default {
           inheritAttrs:false,
           methods: {
               onUpdateName() {}
           }
       }
   </script>
   
   // childCom.vue
   <template>
   	{{ name }} // grandParentName
   </template>
   <script>
       export default {
           props:['name'],
           methods:{
               handleUpadate() {
                   this.$emit("updateName", "updateName-Kevin");
               }
           }
       }
   </script>
   ```

3. ref

   - 适用于 父子组件、祖孙组件

   ```vue
   // parentCom.vue
   <template>
   	<childCom ref="childComRef"></childCom>
   </template>
   <script>
   	export default {
           methods:{
               getChild(){
                   this.$refs.name
               }
           }
       }
   </script>
   ```

   

4. $parent + $children +$emit 

   - 适用于 父子组件、兄弟组件

   ```vue
   // parentCom.vue
   <template>
   	<childCom1></childCom1>
   	<childCom2></childCom2>
   </template>
   <script>
   	export default {
           data(){
               return {
                   count: 666
               }
           }
       }
   </script>
   
   // childCom1
   <template>
   	{{ $parent.count}}
   </template>
   <script>
   	export default {
          
       }
   </script>
   
   // childCom2
   <template>
   	{{ $parent.count}}
   </template>
   <script>
   	export default {
    		
       }
   </script>
   ```

   

5. provide + inject

   - 适用于 父子组件、祖孙组件

   - 在Vue2.x中默认不是响应式的，``app.config.unwrapInjectedRef = true`` 以保证注入会自动解包这个计算属性，这将会在 Vue 3.3 后成为一个默认行为。

   ```vue
   // grandParentCom.vue
   <template>
   	<parentCom></parentCom>
   </template>
   <script>
       export default {
           data(){
               return {
             		message:"i am parentCom"      
               }
           },
        	provide(){
               return {
                   message: computed(() => this.message)
               }
           }
       }
   </script>
   
   // parentCom.vue
   <template>
   	<section>
       	<span>{{ message }}</span> // 'i am parentCom'
   		<childCom></childCom>
       </section>
   </template>
   <script>
       export default {
           inject:['message'] 
       }
   </script>
   
   // childCom.vue
   <template>
   	<span>{{ message }}</span> // 'i am parentCom'
   </template>
   <script>
       export default {
           inject:['message'] 
       }
   </script>
   ```

   

6. EventBus

   - 适用于 兄弟组件传值

   ```js
   // 创建一个中央时间总线类  
   class Bus {  
     constructor() {  
       this.callbacks = {};   // 存放事件的名字  
     }  
     $on(name, fn) {  
       this.callbacks[name] = this.callbacks[name] || [];  
       this.callbacks[name].push(fn);  
     }  
     $emit(name, args) {  
       if (this.callbacks[name]) {  
         this.callbacks[name].forEach((cb) => cb(args));  
       }  
     }  
   }  
     
   // main.js  
   Vue.prototype.$bus = new Bus() // 将$bus挂载到vue实例的原型上  
   // 另一种方式  
   Vue.prototype.$bus = new Vue() // Vue已经实现了Bus的功能  
   ```

   ```vue
   // parentCom.vue
   <template>
   	<childCom1></childCom1>
   	<childCom2></childCom2>
   </temaplate>
   
   // childCom1.vue
   <script>
   export default {
       data() {
       	return {
               msg:"msgmsgmsg"
           }  
       },
       methods:{
           sendMsg() {
               this.$bus.emit('changeMessage',this.msg)
           }
       }
   }
   </script>
   
   // childCom1.vue
   <script>
   export default {
       data() {
       	return {
               msg:"msgmsgmsg"
           }  
       },
       created() {
         this.$bus.$on('foo', this.setMsg)    
       },
       methods:{
           setMsg(msg) {
              	this.msg = msg
           }
       }
   }
   </script>
   ```

7. VueX / Pinia

   - 适用于 所有场景