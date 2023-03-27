# 基于Koa搭建一个基于ESmodule的静态服务器

## 总体思路

1. 处理第三方模块路径问题
2. 加载静态资源index.html
3. 处理无法编译的文件类型（非js模块）
4. 修改第三方模块的返回路径

## 构建

1. 初始化项目

   - 定义package.json

     ```json
     // package.json
     {
         "name":"vite-cli", // 不要与本地命令冲突，一会儿要用link软链接
     	"version":"1.0.0",
         "main":"index.js",
         "bin":"index.js",
         "dependencies":{
         	"koa":"~",
         	"koa-send":"~", // 处理静态文件，实际可以用"koa-static"代替
             "@vue/compiler-sfc" // 编译.vue文件
     	}
     }
     ```

   - 定义node运行位置

     ```js
     // index.js
     #!/usr/bin/env node
     const Koa = require('koa')
     const send = require("koa-send")
     const app = new Koa()
     ```

2. 创建静态服务器，默认加载index.html

   ```js
   // index.js
   #!/usr/bin/env node
   const Koa = require('koa')
   const send = require("koa-send")
   const app = new Koa()
   
   // 创建静态文件服务器
   app.use(async (ctx,next) => {
       await send(
           ctx,
           ctx.path,
           {root:process.cwd(),index:"index.html"}
       )
       await next()
   })
   app.listen(3000)
   ```

3. link 到npm安装目录

   ```shell
   npm link
   ```

   项目目录下运行``vite-cli``开启

4. 解析类似于``vue`` module路径问题和加载问题

   ```js
   // index.js
   const path = require('path')
   const streamToString = stream => new Promise((resolve,reject) => {
       const chunk = []
       stream.on('data',chunk.push(chunk))
       stream.on('end',() => resolve(Buffer.concat(chunks).toString('utf-8')))
       stream.on('error',reject())
   })
   // 处理第三方模块的路径问题
   // 1.判断请求的文件是否是Js模块
   // 2.将请求中的类似 import vue from 'vue' 变成 import vue from '/@modules/vue'
   app.use(async(ctx,next) => {
       if(ctx.type==='application/javascript'){
           const contents = await streamToString(ctx.body)
           ctx.body = contents
           .replace(/(from\s+['"])(?![\.\]/)//g,'$1/@mdules')
           .replace(/process\.env\.NODE_ENV/g,'"development"') // 因为在浏览器中没有process，所以在打包工具中应该替换路径 
       }
   })
   
   // 加载第三方模块
   // 1.将 ctx.path的路径 类似 /@modules/vue 变成存在的module_modules路径
   app.use(async(ctx,next) => {
       if(ctx.path.startsWith('/@modules/'){
          	const moduleName = ctx.path.substr(10)
   	    const pkgPath = path.join(process.cwd(),'node_modules',moduleName,'package.json')
           const pkg = require(pkgPath)
           ctx.path = path.join('/node_modules',moduleName,pkg.module)
       	await next()
   	})
   })
   ```

5. 处理无法识别的模块(.vue,.css等),运用编译工具

   ```js
   const compilerSFC = require('@vue/compiler-sfc')
   const { Readable } = require('strem')
   
   const streamToString = stream => new Promise((resolve,reject) => {
       const chunk = []
       stream.on('data',chunk.push(chunk))
       stream.on('end',() => resolve(Buffer.concat(chunks).toString('utf-8')))
       stream.on('error',reject())
   })
   const stringToStream = text => {
       const stream = new Readable()
       stream.push(text)
       stream.push(null)
       return stream
   }
   // 编译单文件组件（.vue）
   // 1.判断是否以.vue结尾
   // 2.运用@vue/compiler-sfc进行编译
   // 3.处理第二次请求中的render函数
   app.use(async (ctx,next) => {
       if(ctx.path.endsWith('.vue')){
           const contents = await streamToString(ctx.body)
           // vue2 parse返回的是一个ast对象
           // vue3 parase返回 { descriptor , error}
           const { descriptor } = compilerSFC.parse(contents)
           let code;
           if(!ctx.query.type){
               code = descriptor.script.content
               
               code = code.replace(/export\s+default\s+/g,'const _script =')
               code += `
               	import render as _render from "${ctx.path}?type=template"
   				__script.render = __render
   				export default _script
               `
           }else if (ctx.query.type === 'template'){
               const templateRender = compilerSFC.complierTemplate({source:descriptor.template.content})
               code = templateRender.code
           }
           ctx.type = "application/javascript"
       	ctx.body = stringToStream(code)
       }
       await next()
   })
   ```

   



