## 基础语法

### v-cloak
```
用处：当网络较慢，网页还在加载 Vue.js ，而导致 Vue 来不及渲染，这时页面就会显示出 Vue 源代码。我们可以使用 v-cloak 指令来解决这一问题。
```
### `v-text v-html v-pre`
```
用处  v-text 填充纯文本
	 v-html  填充html片段  会解析html代码
	 v-pre   填充原始信息  
```
### v-once
```
只编译一次   显示内容后即不再具有响应式
```
### v-model
```
实现双向绑定  
```
### 事件修饰符
```
.stop 阻止冒泡  .prevent 阻止默认行为  
.enter 回车键   .delete 删除键  即按下对应的键触发
```
### v-bind 
```
用于属性绑定、样式绑定  可以简写为 ：
```

## 自定义指令

```
Vue.directive('xxx',{mmm:function()})  xxx为指令名   mmm 函数名 是vue自带的
自定义指令可以带参数  即function 可以带参数
```

## 计算属性(computed)

```
包含多个方法的对象
对状态属性进行计算返回一个新的数据, 供页面获取显示
一般情况下是相当于是一个只读的属性
利用set/get方法来实现属性数据的计算读取, 同时监视属性数据的变化
如何给对象定义get/set属性
	在创建对象时指定: get name () {return xxx} / set name (value) {}
  	对象创建之后指定: Object.defineProperty(obj, age, {get(){}, set(value){}})
```

## 方法(methods)

```
包含多个方法的对象
供页面中的事件指令来绑定回调
回调函数默认有event参数, 但也可以指定自己的参数
所有的方法由vue对象来调用, 访问data中的属性直接使用this.xxx
```

#### computed和methods的区别

```
计算属性是基于它们的依赖进行缓存的  即 当多次请求 数据不变化是计算属性指挥调用一次   而方法每次都会调用
方法不存在缓存
```

## 侦听器(watch)

```
使用场景：数据变化式执行异步或开销较大的操作
包含多个属性监视的对象
分为一般监视和深度监视
    xxx: function(value){}
	xxx : {
		deep : true,
		handler : fun(value)
	}
另一种添加监视方式: vm.$watch('xxx', function(value){})
```

### 过滤器（filter）

```
作用：格式化数据 比如 将字符串数据全部格式化成首字母大写   
 自定义过滤器  Vue.filter('xxx',function{})  xxx为过滤器名称     function可以有形参
 使用 msg|xxx eg: </div>{{msg|upper}}</div>
```

# 组件

## 组件注册

```
语法：Vue.compoent(xxx,{data:组件数据，templete:组件模板内容})

注意事项：data必须是一个函数；
		组件模板内容必须是单个跟元素
		组件模板内容可以是模板字符串（需要浏览器支持）  即 ` `
如果使用驼峰式命名组件，那么在使用组件的时候，只能在字符串模板中用驼峰的方式使用组件，但是在普通的标签模板中,必须使用短横线的方式使用组件        
       
局部组件：vue实例中有一个components参数 
```
## 组件间数据交互

###  父组件向子组件传值

```
props  用于接受父组件传递给子组件的值   :xxx:"xxx" 父组件传值个子组件的方式
props命名规则：在props中使用驼峰形式，模板中需要使用短横线的形式字符串形式的模被中没有这个限制
```

### 子组件向父组件传值

```
子组件通过自定义事件向父组件传递信息
父组件监听子组件的事件
```

### 非父子组件间传值

```
单独的事件中心管理组件间的通信	 eg： var eventHUb = new Vue()
监听事件与销毁事件		监听 eventHUb.$on('add-todo',addtodo) 销毁 eventHUb.$on('add-todo')
触发事件					eventHUb.$emit('add-todo')
```

### 插槽

```
作用：父组件向子组件传递内容  内容指的是模板内容
```

#### 具名插槽

```

```

#### 作用域插槽

```
应用场景:父组件对子组件的内容进行加工处理
```
## 前后端交互
### fetch

```
语法结构：fetch(url).then(fn2).then(fn3)
```

#### 请求参数

```
1.常用配置选项
method(String):HTTP请求方法，默认为GET(GET、POST、PUT、DELETE)
body(String): HTTP的请求参数
headers(Object):HTTP的请求头，默认为8
```

### axios

```
是一个基于Promise用于浏览器和node.js的HTTP客户端。
```
#### 特性
```
支持浏览器和node.js
支持promise
能拦截请求和响应
自动转换JSON数据
eg： axios.get(url).then(callback )
```

#### 参数传递

```
post请求  默认传递的是json格式
```

#### axios全局配置

```
axios.defaults.timeout = 3000;/超时时间
axios.defaults.baseURL = http://localhost:3000/app;//默认地址
axios.defaults.headers['mytoken’]= 'aqwerwqwerqwer2ewwe23eresdf23’//设置请求头
```

#### 拦截器

##### 请求拦截器

```
语法：axios.interceptors.request.use(function(){}).then()
eg:
//添加一个请求拦截器
axios.interceptors.request.use (function ( config){//在请求发出之前进行一些信息设置
return config;}, function (err) {//处理响应的错误信息}) ;
```

##### 响应拦截器

```
语法：axios.interceptors.response.use(function(){}).then()
```
## 前端路由

### 概念

```
概念:根据不同的用户事件，显示不同的页面内容
本质:用户事件与事件处理函数之间的对应关系
SPA (Single Page Application）单页面应用程序:整个网站只有一个页面，内容的变化通过Ajax局部更新实现、同时支持浏览器地址栏的前进和后退操作
SPA实现原理之一:基于URL地址的hash (hash的变化会导致浏览器记录访问历史的变化、但是hash的变化不会触发新的URL请求)
```

### Vue Router(路由管理器)
```
1、引入相关库文件 
2、添加路由连接   
		router-linke 是vue中提供的标签  默认会被渲染成<a>标签
		to属性默认渲染成href属性
		to属性的值会被渲染成以#开头的hash值
3、路由填充位(路由占位符)  
		<!--将来通过路由规则匹配到的组件，将会被渲染到router-view所在的位置-->
		<router-view><router-view>
4、定义路由组件
5、配置路由规则并创建路由实例
 eg：//创建路由实例对象
	var router = new vueRouter ( {
	 //routes是路由规则数组
	routes: [
	//每个路由规则都是一个配置对象，其中至少包含path和component两个属性:ll path表示当前路由规则匹配的	//hash地址
	//component表示当前路由规则对应要展示的组件
	{path : ' /user ' , component: User} ,
	{path: ' /register ' , component: Register}]
	})
5、把路由挂在到Vue根实例上
```

### 路由重定向

```
路由重定向指的是:用户在访问地址A的时候，强制用户跳转到地址c，从而展示特定的组件页面;
			通过路由规则的redirect 属性，指定一个新的路由地址，可以很方便地设置路由的重定向;
```
### 嵌套路由
```
在路由规则里添加children属性 ，在children属性里在设置路由
eg { path: "/user', component: user}
	{path: "/ register',component: Register},
	//通过children属性,为/register添加子路由规则
	children: [
	{ path: '/register/tab1',component: Tab1 },
	{ path: " /register/tab2' ,component: Tab2 }]
```

### 动态路由匹配

```
应用场景:通过动态路由参数的模式进行路由匹配
  eg:{ path : '/user/:id ', component : User }  :id就是动态参数
```

### 路由组件传递参数

```
$route与对应路由形成高度耦合，不够灵活，所以可以使用props将组件和路由解耦
eg {path:'/user/:id'component:user,props: { uname : 'lisi'，age: 12 }}
	const User = {
	props: [ "uname " , " age '],
	tempiate: '<div>用户信息:{ { uname + '---' t age} }</div>'
	}
```
### 命名路由

```
const User = {
props: [ "uname " , " age '],
tempiate: '<div>用户信息:{ { uname + '---' t age} }</div>'
```
## 模块化开发

```
浏览器端的模块化规范 CMD AMD 
服务器端的模块化规范 CommonJS
					模块分为单文件模块与包
					模块成员导出: module.exports和exports模块
					成员导入: require('模块标识符")
```

## webpack

```
webpack是一个流行的前端项目构建工具(打包工具)，可以解决当前web开发中所面临的困境。
webpack提供了友好的模块化支持，以及代码压缩混淆、处理js,兼容问题、性能优化等强大的功能，从而让程序员把工作的重心放到具体的功能实现上，提高了开发效率和项目的可维护性。
```

### 加载器

#### 通过loader打包非js模块

```
在实际开发过程中，webpack默认只能打包处理以.js后缀名结尾的模块，其他非.js后缀名结尾的模块，webpack 默认处理不了，需要调用loader 加载器才可以正常打包，否则会报错!
```

##### 常见loader加载器

```
less-loader 可以打包处理.less相关的文件
sass-loader 可以打包处理.scss相关的文件
url-loader可以打包处理css中与url路径相关的文件
```

##### 配置postCSS自动添加css的兼容前缀

```
运行npm i postcss-loader autoprefixer -D命令
在项目根目录中创建postcss 的配置文件 postcss.config.js，并初始化如下配置:
在webpack.config.js 的module -> rules 数组中，修改css的 loader 规则
```

### Vue单文件组件

```
组成 template 模板区域   script 业务逻辑区域   style样式区域
```

## Vue脚手架

``` 
官方网站  https://cli.vuejs.org/zh/
```

### 基本使用

```
创建项目  1、交互式命令行 vue creact xxx   2、基于图像 vue ui
```

### Element-Ul

```
官网： http://element-cn.eleme.io/#/zh-CN
```

### 项目总结

#### axios请求拦截器

```
 eg:通过axios请求拦截器添加token，保证拥有获取数据的权限。
 	axios.interceptors.request.use(config=>{
	为请求头对象，添加Token 验证的 Authorization字段
	config.headers.Authorization = window.sessionstorage.getitem ( 'token ')return config
})
```

### 路由懒加载

```
当路由被访问时才会加载对应组件  
详细文档  https://router.vuejs, org/zh/guide/advanced/lazy-loading.html
步骤
①安装@babel/plugin-syntax-dynamic-import包。
在babel.config.js配置文件中声明该插件。
将路由改为按需加载的形式，示例代码如下:
const Foo = () => import ( /* webpackChunkName: "group-foo" */ './Foo. vue ')
const Bar = () => import ( /* webpackChunkName: "group-foo"* / './Bar.vue ')

```

### 项目上线

#### 1.通过node创建web服务器

#### 2.开启gzip 配置

```
特点： 使用gzip可以减小文件体积，使得传输速度更快
步骤：可以通过服务器端使用Express做gzip压缩。其配置如下:
		安装gzip  npm install compression -D
		导入包    const compression = require('compression')
		使用中间件   app.use(compression())  //一定要写到静态资源托管之前
```



#### 3．配置https服务

```
1、申请SSL    	免费的ssl申请网站 https://freessl.cn/官网，输入要申请的域名并选择品牌。
2、在后台项目导入证书
```



#### 4.使用pm2管理应用 

```
安装 npm i pm2 -g
启动项目  pm2 start 脚本 --name 自定义名称
查看运行项目 plm2 ls
重启项目 pm2 restart 自定义名称
停止项目 pm2 stop 自定义名称
停止项目 pm2 delete 自定义名称
```

