# socket.io 基本使用(Node)

## 概述

socket.io是基于websocket封装的库。

## 引入

- 客户端

  ```JS
  const io = require('socket.io-client')
  ```

- 服务器

  ```js
  const io = require('socket.io')
  ```

## 创建实例

- 客户端

  ```js
  const express = require('express');
  const app = express()
  const { createServer } = require("http");
  const { Server } = require("socket.io")
  const httpServer = createServer(app);
  const io = new Server(httpServer,{
     	/* options */
  })
  httpServer.listen(port, () => {
      
  })
  ```

- 服务器

  ```js
  import { io } from 'socket.io-client'
  const socket = io(host,{
  	/* options */
      
      transports:["webscoket","polling"] // 连接方式
      
      autoConnect:false, // 是否自动连接，默认true，设置false后可connect() 或者 open()手动开启。
      reconnection:false, // 是否自动重连，默认true，设置false后需手动重连
      auth:{
          token:''
      }
  })
  ```

## 销毁断开连接

- 服务端

  ```js
  // 关闭所有的连接
  io.disconnectSockets();
  // 关闭指定房间/socket的连接
  io.in("room1").disconnectSockets(true);
  ```

  

## 命名空间

默认都是在``/``空间下通信

- 服务端

  ```js
  // 调用 of 方法来创建新的命名空间
  const myNameSpcaeIo = io.of('/my-namespace')
  myNameSpcaeIo.on('connection',socket => {
      
  })
  ```

- 客户端

  ```js
  const clientIo = io('/my-namespace')
  ```



## 频道Room

- 服务端

  ```js
  // 订阅频道
  io.connection('connection',socket => {
      socket.join('any room')
  })
  io.in(SocketId).socketsJoin("room1")
  // 离开频道
  io.connection('quit',socket => {
      socket.leave('any room')
  })
  // 频道广播
  io.to('any romm').emit('some message')
  
  // 批量转入
  io.socketsJoin('room1')
  io.in("room1").socketsJoin(["room1","room2"])
  // 批量离开
  io.socketsLeave("room1");
  io.in("room1").socketsLeave(["room2", "room3"]);
  ```



## 服务端实例

### io实例

- on

  ```js
  // 建立新连接触发
  io.on("connection",(socket) => {
      
  })
  ```

- to

  ```js
  // 频道广播
  io.to('room1').emit('some message')
  // 多频道广播
  io.to('room1').to('room2').to('room3').emit('some message')
  ```
  
- engine

  - ``io.engine.clientsCount``属性监听已连接服务的客户端数量

  - ``io.engine.on()``监听事件

    ```js
    io.engine.on("initial_headers", (headers, req) => {
      headers["test"] = "123";
      headers["set-cookie"] = "mycookie=456";
    });
    
    io.engine.on("headers", (headers, req) => {
      headers["test"] = "789";
    });
    
    io.engine.on("connection_error", (err) => {
      console.log(err.req);      // the request object
      console.log(err.code);     // the error code, for example 1
      console.log(err.message);  // the error message, for example "Session ID unknown"
      console.log(err.context);  // some additional error context
    });
    ```

### socket实例

- id

- handshake

- Room中的 ``join`` && ``leave``

  ```js
  // 让进入房间
  socket.join('room1')
  // 离开房间
  socket.leave('room1')
  ```

- ``data``

- ``.use()``

  ```js
  socket.use( ([event,...args],next) => {
      next()
  })
  ```

- ``on``

  ```js
  
  // 断开连接前
  socket.on('disconnecting',(socket)=> {
      
  })
  // 断开连接
  socket.on('desconnect',(socket) => {
      
  })
  
  // 收集错误
  socket.on('error',(err) => {
      
  })
  ```

  

## 客户端实例

### socket实例

- ``id``

- ``connected``

  描述连接状态

- ``on``

  ```js
  // 连接成功
  socket.on('connect',() => {
  	    
  })
  
  // 连接错误时
  socket.on('connect_error',(err) => {
     // 如果连接异常，修改transports传输方式
      socket.io.opts.transports = ["polling", "websocket"];
      if(err.message === 'invalid credentials') {
          socket.auth.token = "new token"
          socket.connect()
      }
  })
  
  // 断开连接
  socket.on('disconnect', (reason) => {
     	 // the disconnection was initiated by the server, you need to reconnect manually  
      if (reason === "io server disconnect") {
          socket.connect();
    	}
  })
  ```

  
