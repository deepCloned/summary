## INTERVIEW

**javascript**
### 闭包机制
* 作用域
* 作用域链
* 闭包 (立即执行函数 模块化机制)
* 常见场景

### this 机制
* 默认绑定 -- 普通的函数调用
* 隐式绑定 -- 某一个对象访问一个方法 obj.getName() -- this 指向距离方法最近的对象
* 显式绑定 -- call apply bind (三者的区别)
* new 绑定 -- 指向返回的新对象
* 箭头函数

### prototype 机制
* [[prototype]] __proto__
* prototype
* constructor
* Object.create(obj)
* isProrotypeOf
* setPrototypeOf
* 实现继承的几种方式
  - 共享原型 Bar.
* JavaScript 中的实例化与传统 class 定义的类实例有什么区别
  - javaScript 通过 prototype 建立两个对象直接的连接，使 a 对象可以访问 b 对象身上的属性和方法
  - 传统的类 实例是复制了一份父类的属性和方法

### ajax
* ajax 的简单封装 XMLHttpRequest
* http tcp 协议（常见状态码）
* 浏览器缓存机制 (localStorage sessionStorage(对应服务端session) cookie)

### wx.request axios 的封装

*****************************************************

**html+css**

### 垂直居中的方式
* display: flex; align-items: center; justify-content: center;  -- flex 布局
* position: relative; => position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%, 0)
* grid
* table

****************************************************

**jQuery**

### 基础
* 如何实现无 new 实例化 jQuery 对象
* 如何实现链式调用

*****************************************************

**Vue**



*****************************************************

**细节点**
* 关于请求
  - 请求成功：正确的返回了数据
  - 请求成功：正确的数据为空
  - 请求成功：没有相关数据
  - 请求失败
    - 服务器错误
    - 请求参数错误 -- 这两种情况需要对页面展示做一些处理

### element-ui

### vue-element-admin

### express
* 路由
* 什么是中间件 -- 函数 在特定时间调用的函数
* 各种常用中间件
* 路由的拆分 + 注册
* 全局异常的处理
* jwttoken 的生成、过期的处理
* 接口权限的设置