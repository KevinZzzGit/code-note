# vue-router基本使用

## 概述

vue-router实际上是封装了路由功能的Vue插件。

## 引入和挂载

- CDN引入

  ```html
  <script src="https://unpkg.com/vue-router@4"></script>
  ```

- 项目引入

  ```javascript
  import { createRouter, createWebHistory } from 'vue-router'
  const routes = [  
      { path: '/', component: Home },  
      { path: '/about', component: About }
  ]
  const router = createRouter({
      history:createWebHistory(),
      routes:routes
  })
  const app = new Vue({
      router
  }).$mount('#app')
  ```

## 路由标签

###  router-view

```vue
<div id="app">
    <!-- 路由出口 -->
    <router-view></router-view>
</div>
```

### router-link

```vue
<!-- <router-link> 默认会被渲染成一个 `<a>` 标签 -->
<router-link to="/foo">Go to Foo</router-link>
```

- name

  命名视图，用来在同级展示多个视图

  ```vue
  <router-view class="view one"></router-view>
  <router-view class="view two" name="a"></router-view>
  <router-view class="view three" name="b"></router-view>
  ```

  ```js
  const router = new VueRouter({
    routes: [
      {
        path: '/',
        components: {
          default: Foo,
          a: Bar,
          b: Baz
        }
      }
    ]
  })
  ```



## 路由模式

### 哈希(hash)模式

```vue

```



### 历史(History)模式



##  路由匹配

vue-router使用 [path-to-regexp](https://github.com/pillarjs/path-to-regexp/tree/v1.7.0)作为匹配引擎。优先级依照队列原则。**以 `/` 开头的嵌套路径会被当作根路径。 这让你充分的使用嵌套组件而无须设置嵌套的路径。**

### 动态路由匹配

一个“路径参数”使用冒号 `:` 标记。当匹配到一个路由时，参数值会被设置到 `this.$route.params`

```js
const router = new VueRouter({
  routes: [
    // 动态路径参数 以冒号开头
    { path: '/user/:id', component: User }
  ]
})
```



## 嵌套路由

```js
const router = new VueRouter({
  routes: [
    {
      path: '/user/:id',
      component: User,
      children: [
        {
          // 当 /user/:id/profile 匹配成功，
          // UserProfile 会被渲染在 User 的 <router-view> 中
          path: 'profile',
          component: UserProfile
        },
        {
          // 当 /user/:id/posts 匹配成功
          // UserPosts 会被渲染在 User 的 <router-view> 中
          path: 'posts',
          component: UserPosts
        }
      ]
    }
  ]
})
```



## 路由导航

### 声明式导航

```vue
<router-link to="/path"></router-link>
<router-link :to="{path:'/path',query:{name:'foo'}}"></router-link>
```

### 编程式导航

- 使用path会自动忽略params,若要用params传参,必须在路由对象中添加name属性,以供使用

```js
this.$router.push({
    path: "路由路径"
    name: "路由名",
    query: {
        "参数名": 值
    }
    params: {
        "参数名": 值
    }
})
```



## Router和Route实例

当Router挂载时会将Router和Route同时挂载到App上

### Router

- ``mode``
- ``base``
- ``routes``

- ``push()``
- ``go()``
- ``replace()``
- ``back()``
- ``forward()``
- ``push()``

- ``scrollBehavior()``

```js
  scrollBehavior (to, from, savedPosition) {
    // return 期望滚动到哪个的位置
  }


// 只需将 behavior 选项添加到 scrollBehavior 内部返回的对象中，就可以为支持它的浏览器 (opens new window)启用原生平滑滚动：
scrollBehavior (to, from, savedPosition) {
  if (to.hash) {
    return {
      selector: to.hash,
      behavior: 'smooth',
    }
  }
}
```



### Route

- ``name``

  命名路由，路由导航时可通过名字进行跳转

  ```js
  const router = new VueRouter({
    routes: [
      {
        path: '/user/:userId',
        name: 'user',
        component: User
      }
    ]
  })
  router.push({ name: 'user', params: { userId: 123 } })
  ```

- ``path``

- ``component``

- ``params``

- ``query``

- ``props``

- ``meta``

  ```js
  // 我们需要遍历 $route.matched 来检查路由记录中的 meta 字段。
  router.beforeEach((to, from, next) => {
    if (to.matched.some(record => record.meta.requiresAuth)) {
      // this route requires auth, check if logged in
      // if not, redirect to login page.
      if (!auth.loggedIn()) {
        next({
          path: '/login',
          query: { redirect: to.fullPath }
        })
      } else {
        next()
      }
    } else {
      next() // 确保一定要调用 next()
    }
  })
  ```

- ``redirect``

  路由重定向

  ```js
  const router = new VueRouter({
    routes: [
      { path: '/a', redirect: '/b' },
      { path: '/b', redirect: { name: 'foo' } },
      { path: '/c', redirect: to => {
        // 方法接收 目标路由 作为参数
        // return 重定向的 字符串路径/路径对象
      }}
    ]
  })
  ```



## 路由守卫(Hooks)

- **`to: Route`**: 即将要进入的目标 [路由对象](https://v3.router.vuejs.org/zh/api/#路由对象)

- **`from: Route`**: 当前导航正要离开的路由

- **`next: Function`**: 一定要调用该方法来 **resolve** 这个钩子。执行效果依赖 `next` 方法的调用参数。

  - **`next()`**: 进行管道中的下一个钩子。如果全部钩子执行完了，则导航的状态就是 **confirmed** (确认的)。
  - **`next(false)`**: 中断当前的导航。如果浏览器的 URL 改变了 (可能是用户手动或者浏览器后退按钮)，那么 URL 地址会重置到 `from` 路由对应的地址。
  - **`next('/')` 或者 `next({ path: '/' })`**: 跳转到一个不同的地址。当前的导航被中断，然后进行一个新的导航。你可以向 `next` 传递任意位置对象，且允许设置诸如 `replace: true`、`name: 'home'` 之类的选项以及任何用在 [`router-link` 的 `to` prop](https://v3.router.vuejs.org/zh/api/#to) 或 [`router.push`](https://v3.router.vuejs.org/zh/api/#router-push) 中的选项。
  - **`next(error)`**: (2.4.0+) 如果传入 `next` 的参数是一个 `Error` 实例，则导航会被终止且该错误会被传递给 [`router.onError()`](https://v3.router.vuejs.org/zh/api/#router-onerror) 注册过的回调。

### 全局守卫

```js
const router = new VueRouter({ ... })
// 全局前置守卫
router.beforeEach((to, from, next) => {
  // ...
})

// 全局后置钩子
router.afterEach((to, from) => {
  // ...
})
```

### 局部守卫

```js
const router = new VueRouter({
  routes: [
    {
      path: '/foo',
      component: Foo,
      beforeEnter: (to, from, next) => {
        // ...
      },
      beforeRouteEnter(to, from, next) {
      // 在渲染该组件的对应路由被 confirm 前调用
      // 不！能！获取组件实例 `this`
      // 因为当守卫执行前，组件实例还没被创建
     },
     beforeRouteUpdate(to, from, next) {
     // 在当前路由改变，但是该组件被复用时调用
     // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
     // 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
     // 可以访问组件实例 `this`
     },
     beforeRouteLeave(to, from, next) {
     // 导航离开该组件的对应路由时调用
     // 可以访问组件实例 `this`
       }
     }
  ]
})
```

###  完整的导航解析流程

1. 导航被触发。
2. 在失活的组件里调用 `beforeRouteLeave` 守卫。
3. 调用全局的 `beforeEach` 守卫。
4. 在重用的组件里调用 `beforeRouteUpdate` 守卫 (2.2+)。
5. 在路由配置里调用 `beforeEnter`。
6. 解析异步路由组件。
7. 在被激活的组件里调用 `beforeRouteEnter`。
8. 调用全局的 `beforeResolve` 守卫 (2.5+)。
9. 导航被确认。
10. 调用全局的 `afterEach` 钩子。
11. 触发 DOM 更新。
12. 调用 `beforeRouteEnter` 守卫中传给 `next` 的回调函数，创建好的组件实例会作为回调函数的参数传入。

## 路由懒加载

```js
// 使用动态 import (opens new window)语法来定义代码分块点 (split point)：
const Foo = () => import('./Foo.vue')
const router = new VueRouter({
  routes: [{ path: '/foo', component: Foo }]
})

// webpackChunkName:按组分块
const Foo = () => import(/* webpackChunkName: "group-foo" */ './Foo.vue')
const Bar = () => import(/* webpackChunkName: "group-foo" */ './Bar.vue')
const Baz = () => import(/* webpackChunkName: "group-foo" */ './Baz.vue')
```

## 导航故障

### 检测故障

```js
import VueRouter from 'vue-router'
const { isNavigationFailure, NavigationFailureType } = VueRouter

// 正在尝试访问 admin 页面
router.push('/admin').catch(failure => {
  if (isNavigationFailure(failure, NavigationFailureType.redirected)) {
    // 向用户显示一个小通知
    showToast('Login in order to access the admin panel')
  }
})
```

- `NavigationFailureType` 

  可以帮助开发者来区分不同类型的*导航故障*。有四种不同的类型：

  - `redirected`：在导航守卫中调用了 `next(newLocation)` 重定向到了其他地方。

  - `aborted`：在导航守卫中调用了 `next(false)` 中断了本次导航。

  - `cancelled`：在当前导航还没有完成之前又有了一个新的导航。比如，在等待导航守卫的过程中又调用了 `router.push`。

  - `duplicated`：导航被阻止，因为我们已经在目标位置了。

- ``Failure``

  - ``to``：失败导航的目标位置

  - ``from``:失败导航的当前位置
