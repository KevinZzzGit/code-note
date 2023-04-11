# 对接网易七鱼Web端

## 1、前台

- 初始化

  实例化``ysf``并挂载到全局对象，由后台生成

  ```js
  (function (w, d, n, a, j) {
  	w[n] = w[n] || function () { return (w[n].a = w[n].a || []).push(arguments)};
  	j = d.createElement('script');
  	j.async = true;
  	j.src = 'https://qiyukf.com/script/044865c94981c048609d5c94c1ae9c6d.js';
  	d.body.appendChild(j);
  })(window, document, 'ysf');
  ```

- 配置

  ```JS
  ysf('config', {
      uid: "123456789",  // 用户Id
      name: 'test',     // 用户名称
      email: 'test@163.com', // 用户邮箱
      mobile: '13888888888',  // 用户电话
      staffid: '123', // 客服id
      groupid: '123' // 客服组id
  })
  ```

  

- API基础使用

  ```js
  // 判断是否加载完成
   ysf('onready', function () {
    
  })
  // 登入
  ysf('open')
  //登出
  ysf('logoff')
  // 监听到未读消息
  ysf('onunread', function(data) {
  	// todo
       console.log(data);
  });
  // 埋点
  ysf('track', 'clickAd', {
  	userId: '12345'
  })
  ```

  

## 2、后端

### 2.1  官方管理后台

### 2.2 客户端api对接

