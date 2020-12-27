### JSX

```
react定义的类似xml的js拓展语法： xml+js
全称：javascrip XML
作用：用来创建react虚拟dom(元素)对象
babel.js作用：
浏览器不能直接解析JSX代码，需要babel转译为纯JS的代码才能运行只要用了JSx，都要加上 type="text/babel"，声明需要babel来处理
```

#### JSX语法规则

```
1.定义虚拟DOM时，不要写引号。
2.标签中混入Js表达式时要用{。
3.样式的类名指定不要用class，要用className.
4.内联样式，要用style=i{key: value}}的形式去写。
5.只有一个根标签
6.标签必须闭合7.标签首字母
 (1).若小写字母开头，则将改标签转为html中同名元素，若html中无该标签对应的同名元素，则报错。
 (2).若大写字母开头，react就去渲染对应的组件，若组件没有定义，则报错。
```

### XML

```
早期用于存储和传输数据
```

### 虚拟Dom

```
本质：object类型的对象
比较'轻' 因为虚拟dom是react内部在用 没有真是dom那么多属性
 虚拟dom最终会被React转化为真实DOM，呈现在页面上
```

#### 渲染

```
语法：ReactDOM.render(dom1,dom2)  
      dom1 创建的虚拟dom
      dom2 真是的dom容器
作用 将虚拟dom渲染到页面中
```

#### 创建方式

```
纯js方式/jsx
```

### 函数式组件和类式组件的区别

```
语法上，
	函数组件是一个纯函数，它接收一个props对象返回一个react。
	元素。而类组件需要去继承React.Component,并且创建render函数返回react
状态管理
	函数组件是一个纯函数，你不能在组件中使用setstate()
生命周期函数
	函数式组件不能使用生命周期钩子  react16.8之后可以使用hooks中的useState管理状态
```



## 三大属性

```
state是组件对象最重要的属性，值是对象(可以包含多个数据)
组件被称为"状态机"，通过更新组件的state来更新对应的页面显示(重新渲染组件)
编码操作：初始化状态  读取某个状态值   更新状态(this.setState)
		constructor(prop){    const a = this.state.xxx   
			super(props)
			//初始化状态
			this.state = {
			xxx
			}
		}
注意：新添加方法:内部的this默认不是组件对象，而是undefined 
解决方案(bind)  eg this.xxx = this.xxx.bind(this)
```

```
props   父组件与子组件交互的唯一方式
2)对props中的属性值进行类型限制和必要性限制
props-types
默认值
Person.default
```

```
refs
React.createRef()调用后可以返回一个容器，该容器可以存储被ref所标识的节点
		一个容器只能存一对数据  新数据回覆盖旧数据
```

### 字符组件传值

```
父传子 eg 父组件传值 <List todos={this.state.todos} /> 子组件接受 const { todos } = this.props;
子组件不能直接改变父组件的状态 通过调用父组件中的方法	
```

### 生命周期

```
1、第一次初始化渲染显示: ReactDOM.render()
	*constructor():创建对象初始化state
	* componentWillMount():将要插入回调
	*render():用于插入虚拟DOM回调
	* componentDidMount():已经插入回调
2、每次更新state: this.setSate()
	*componentWillUpdate():将要更新回调
	*render():更新(重新渲染)
	*componentDidUpdate():己经更新回调
3、移除组件:ReactDOM.unmountComponentAtNode(containerDom)
	*componentWillUnmount():组件将要被移除回调
```

#### 新增生命周期

```
getDerivedStateFromProps 适用于特殊例子 即 state的值在任何时候都取决于props
						 派生状态会导致代码冗余，使组件难以维护

区别：取消了三个will相关生命周期   
	 新增了getDerivedStateFromProps getSnapshotBeforeUpdate
```

### Diffing算法

```
数组遍历渲染时候  key值建议使用 id之类的唯一标识作为key
```

### react脚手架代理

```
作用：同源政策 解决跨域问题 
优点:可以配置多个代理，可以灵活的控制请求是否走代理
缺点:配置繁琐，前端请求资源时必须加前缀。
```

#### proxy

```
直接在package.json追加一下配置
  "proxy":'http://localhost:3000'
优点：配置简短，请求资源时可以不加任何前缀
缺点：不能配置多个代理
```

#### 设置配置文件

```
在src下创建配置文件 src/setupProxy.js
```

```
配置如下：
const proxy = require( ' http-proxy-middleware ')
module.exports = function(app) i
app.use(
proxy( ' /api1'，i //api1是需要转发的请求(所有带有/api1前缀的请求都会转发给5000)
target: 'http:/ /localhost: 5000'，//配置转发目标地址(能返回数据的服务器地址)
changeorigin: true，//控制服务器接收到的请求头中host字段的值
/*
changeorigin设置为true时，服务器收到的请求头中的host为: localhost : 5000changeorigin设置为false时，服务器收到的请求头中的host为: loca7host : 3000changeorigin默认值为false，但我们一般将changeorigin值设为true
*/
pathRewrite:{'^/api1':""}//去除请求前缀，保证交给后台服务器的是正常请求地址(必须配置))，
proxy( ' /api2'，{
target: 'http: / /localhost : 5001',changeorigin: true,
pathRewrite: { '^/api2 ': ''}
```



### 常用ajax库

```
jquery  比较重 不建议使用
axios 轻量级 a 封装xmlHttpRequest对象的ajax  b promise风格  c 可以在浏览器端和node服务使用
fetch 原生函数 老版本浏览器不支持
	  不再使用xmlHttpRequest对象提交ajax请求
	   为了兼容低版本的浏览器 可以引入兼容库fetch.js
```

### 组件键通信

```
props 可以传递函数和一般数据 只能一层一层传递
		一般数据->父组件传递数据给子组件->子组件读取数据
		函数数据->子组件传递数据给父组件->子组件调用数据
消息订阅与发布  一般用于兄弟组件或多级组件
 工具 PubSubJS
redux
```

### 前端路由的实现

####  history库

```
a.网址: https://github.com/ReactTraining/history
b.管理浏览器会话历史(history)的工具库
c.包装的是原生BOM中 window.history和 window.location.hash
```

#### 路由

```
 <BrowerRouter>、<HashRouter> 浏览器路由
 <Route> 路由
 <Switch> 切换路由
 <NavLink> 导航路由连接  
 <Rederedict> 重定向路由
```

##### BrowerRouter和HashRouter区别

```
底层原理不一样:
	BrowserRouter使用的是H5的history API，不兼容IE9及以下版本。
	HashRouter使用的是URL的哈希值。
ur1表现形式不一样
	BrowserRouter的路径中没有#,
	例如: localhost:3000/demo/testHashRouter的路径包含# ,
	例如: localhost:300/#/demo/test
刷新后对路由state参数的影响
	BrowserRouter没有任何影响，因为state保存在history对象中。
	HashRouter刷新后会导致路由state参数的丢失。
备注: HashRouter可以用于解决一些路径错误相关的问题。
```



#### 向路由组件传递数据

```
1.params参数
	路由链接(携带参数): <Link to='/demo/test/tom/18'}>详情</Link>
	注册路由(声明接收):<Route path="/demo/test/ :name/:age" component={Test}/>
	接收参数: this.props.match.params
2.search参数
	路由链接(携带参数):<Link to='/demo/test?name=tom&age=18'}>详情</Link>
	注册路由(无需声明，正常注册即可):<Route path="/demo/test" component={Test}/>	 接收参数: this.props.location.search
	备注:获取到的search是urlencoded编码字符串，需要借助querystring解析
3.state参数
	路由链接(携带参数):
    <Link to={{path:'/demo/test',state:{name :'tom' ,age:18}}}>详情</Link>
    注册路由(无需声明，正常注册即可):
    <Route path=" /demo/test" component={Test}/>
	接收参数: this.props.location.state
	备注:刷新也可以保留住参数
```

#### 路由组件与一般组件的区别

```
1.写法不同:
	一般组件: <Demo/>
	路由组件:<Route path=" /demo" component={Demo}/>
2.存放位置不同:
	一般组件: components
	路由组件: pages
3.接收到的props 
	一般组件: 写组件标签时传递了什么，就能收到什么
	路由组件: 接收到三个固定的属性 (history,location,match)
```



#### antd相关的按需导入配置

```
下载依赖包
	yarn add react-app-rewired --dev
	yarn add babel-plugin-import --dev
修改package.json
"scripts": {
	"start": "react-app-rewired start",
	"build": "react-app-rewired build",
	"test" : "react-app-rewired test --env=jsdom"
  }
```

#### 按需导入配置

```
yarn add react-app-rewired customize-cra
yarn add babe1-plugin-import
修改package.json
"scripts": {
	"start": "react-app-rewired start",
	"build": "react-app-rewired build",
	"test" : "react-app-rewired test --env=jsdom"
  }
在根目录创建config-overrides.js并进行如下配置
 module.exports = override(
   fixBabelImports('import', {
     libraryName: 'antd',
    libraryDirectory: 'es',
    style: 'css',
  }), );
```



### redux

#### 文档

```
https://www.redux.org.cn/
默认是不能进行异步处理的
```

#### 理解

```
redux是一个独立专门用于做状态管理的JS库(不是react插件库)
它可以用在react, angular, vue等项目中，但基本与react配合使用
作用:管理react应用中多个组件共享的状态
```

#### 流程

```
1.UI发出动作，如click, submit;
2.action,描述发生了什么;
3.reducer处理发生的事情，生成新state;
4.store被更新;
5.UI响应store更新
```

#### 方法

```
createStore 创建包含reduceer的store对象
```
##### store对象
```
作用：redux库最核心的管理对象
内部维护者 state(状态)/reducer(根据旧状态产生一个新状态的函数)
核心方法:getState/dispatch(action)/subscribe(listen)
```
##### applyMiddleware
```
作用：应用上基于redux的中间件(插件库)
```
#####  combineReducers
```
作用： 合并多个reduce函数
```

#### 核心概念

##### action

```
标识要执行行为的对象
包含2个方面的属性:
	type:标识属性，值为字符串，唯一，必要属性
	xxx:数据属性，值类型任意，可选属性
Action.Creator(创建Action的工厂函数)
```

##### reducer

```
根据旧状态产生一个新状态的函数
注意： 返回一个新的状态，不要修改原来的状态 redux中的方法必须时纯函数
纯函数
   一类特别的函数:只要是同样的输入(实参)，必定得到同样的输出(返回)2．必须遵守以下一些约束
   不得改写参数数据
   不会产生任何副作用，例如网络请求，输入和输出设备
   不能调用Date.nowO或者Math.random(等不纯的方法）
```

##### store

```
将state,action 与reduger联系在一起的对象
```

#### react-redux

```
一个react插件 用于简化react应用中使用redux
包含2个重要属性 Provider组件和connect方法
Provider组件 向所有容器组件提供全局的stroe对象 
		eg <Provider store={store}>xxx</Provider
connect方法 包装组件生成容器组件,让被包装组件能与redux进行通信 
		eg connect(mapStateToProps,mapDispatchToProps)(Xxx)
```

##### context理解和使用

```
使用context属性解决多层传递
	context是组件对象的一个属性   值是一个对象
	一个组件指定的context内数据  所有子组件都可读取到
建议尽量不要使用context  可以选择使用redux  redux内部使用的就是context
```
###### 使用

```
创建context容器对象:
	constXXxContext = React.createcontext()
渲染子组时，外面包裹xxxContext.Provider，通过value属性给后代组件传递数据:
	<XXXContext .Provider value={数据}>
		子组件
	</xxxContext.Provider>
后代组件读取数据:
	//第一种方式:仅适用于类组件
	lstatic contextType = xxxContextl/声明接收contextthis.context /读取context	中的value数据
	//第二种方式:函数组件与类组件都可以
	<XxxContext.consumer>
	{
	value = ( l / value就是context中的value数据要显示的内容)
	}
```

##### UI组件

```
只负责UI的呈现，不带有任何业务逻辑
通过props接收数据(一般数据和函数)
不使用任何 Redux的API
一般保存在components文件夹下
```
##### 容器组件

```
负责管理数据和业务逻辑，不负责U的呈现使用Redux的API
一般保存在containers文件夹下
```

##### 相关api

```
Provider组件  使得所有组件都可以得到state数据
connect 用于包装UI组件生成的容器组件 
一般使用两个参数 mapStateToProps mapDispatchToProps
	mapStateToProps 函数   用来确定一般属性
	mapDispatchToProps 对象 用来确定函数(内部使用dispatch方法)属性
  
mapStateToprops() 将外部数据(state对象)转换为UI组件的标签属性
mapDispatchToProps 将分发action的函数转换为UI组件的标签属性
简洁语法可以直接指定为actions对象或包含多个action方法的对象
```

#### redux异步编程

##### redux插件(异步中间件)

```
安装  npm install --save redux-thunk
异步action 返回一个函数 //需要使用redux-thunk
同步action 返回一个对象 //默认只能返回对象
```

##### redux调试插件

```
安装chrome插件
项目内下载工具包依赖 npm install --save-dev redux-devtools-extension
```

##### 常用插件以及为什么要使用

```
applyMiddleware 添加一个中间件 方便数据的处理
thunk 实现react异步处理
createStore 创建包含reduceer的store对象
composeWithDevTools 便于在浏览器中进行调试
combineReducers 把一个由多个不同reducer函数作为value 的object，合并成一个最终的reducer 函数
Provider组件 用provider包裹在根组件外层，使所有的子组件都可以拿到state
```

### antd-mobile实现按需导入以及自定义主题

#### 安装依赖

```
//实现按需打包的依赖
npm install --save-dev babel-plugin-import react-app-rewired
npm install --save-dev customize-cra'
//实现自定义主题的依赖
npm install --save-dev babel-plugin-import less less-loader style-loader css-loader
npm install --save-devless@2.7.3 less-loader 
```

#### 配置文件

```
在根目录下创建config-overrides.js文件 进行以下配置
const {override, fixBabelImports, addLessLoader} = require('customize-cra');
module.exports = override(
    fixBabelImports('import', {
        libraryName: 'antd-mobile',
        libraryDirectory: 'es',
        // style: 'css',
        style: true
    }),
    addLessLoader({
        lessOptions: {
            javascriptEnabled: true,
            //颜色可以视情况改变
            modifyVars: {
            //按钮颜色
              "@brand-primary": "#40a35b",
              //按钮点击时的颜色
              "@brand-primary-tap":"red",
              //文字颜色
              "@color-text-base-inverse": "#ecb439",
            }
        }
    })
);
```

#### package.json部分配置

```
 将原来scripts中的内容替换成以下内容
 "scripts": {
    "start": "react-app-rewired start",
    "build": "react-app-rewired build",
    "test": "react-app-rewired test --env=jsdom",
    "eject": "react-scripts eject"
  },
```

### 导入路由

```
适用于web开发的路由
下载包 npm install --save react-router-dom
```

###  引入redux（不能下载最新版本）

```
npm install --save redux@3.7.2 react-redux redux-thunk 
npm install --save-dev redux-devtools-extension //浏览器调试工具
```

## 拓展

### setState更新状态的两种写法

```
setstate(statechange，[ca11back])------对象式的setstate
	statechange为状态改变对象(该对象可以体现出状态的更改)
	cal1back是可选的回调函数，它在状态更新完毕、界面也更新后(render调用后)才被调用
setstate(updater，[ca11back])------函数式的setstate
	updater为返回statechange对象的函数。
	updater可以接收到state和props。
	cal1back是可选的回调函数，它在状态更新、界面也更新后(render调用后)才被调用。
总结:
	对象式的setstate是函数式的setState的简写方式(语法糖)
	使用原则:
		如果新状态不依赖于原状态===>使用对象方式
		如果新状态依赖于原状态===>使用函数方式
		如果需要在setstate()执行后获取最新的状态数据，
		要在第二个cal1back函数中读取
```

### lazyload

```
使用react里的lazy函数 Suspense等待时自动调用
	import React, {Component,lazy,Suspense} from 'react'
使用懒加载导入组件
	eg: const Home = lazy(()=> import('./Home ' ))
```

### Hooks

```
 Hook是React 16.8.0版本增加的新特性/新语法
可以让你在函数组件中使用state以及其他的React特性
常用Hook
	 state Hook: React.usestate()
	 Effect Hook: React.useEffect()
	 Ref Hook : React.useRef()	
```

#### State Hook

```
state Hook让函数组件也可以有state状态，并进行状态数据的读写操作
	语法: const [xxx，setxxx] = React.useState(initvalue)
useState()说明:
	参数:第一次初始化指定的值在内部作缓存
	返回值:包含2个元素的数组，第1个为内部当前状态值，第2个为更新状态值的函数
setXxx()2种写法:
	setXxx(newvalue):参数为非函数值，直接指定新的状态值，内部用其覆盖原来的状态值
	setXxx(value => newValue):参数为函数，接收原本的状态值，返回新的状态值，内部用其	 覆盖原来的状态值
```
#### Effect Hook
```
 Effect Hook 可以让你在函数组件中执行副作用操作(用于模拟类组件中的生命周期钩子)
 React中的副作用操作:
	发ajax请求数据获取
	设置订阅/启动定时器
	手动更改真实DOM
语法和说明:
	useEffect(o=>{
		//在此可以执行任何带副作用操作
		return ( => {/在组件卸载前执行
        //在此做一些收尾工作，比如清除定时器/取消订阅等}
		}，[statevalue])//如果指定的是门，回调函数只会在第一次render()后执行
可以把useEffect Hook看做如下三个丞数的组合
	componentDidMount()
	componentDidupdate()
	componentwi11Unmount()
```

#### Ref Hook

```
(1). Ref Hook可以在函数组件中存储/查找组件内的标签或任意其它数据
(2)．语法: const refcontainer = useRef()
(3)．作用:保存标签对象,功能与React.createRefO一样
```

### Fragment

```
作用：可以不用有一个真实的Dom根标签 (相当于vue里的<template>)
使用：
	<Fragment></Fragment> 允许传key
	<></>				  不允许
```

#### 组件优化

```
问题
	只要执行setState(),即使不改变状态数据,组件也会重新render()
	只当前组件重新render(就会自动重新render子组件==>效率低
原因：shouldComponentUpdata()总是返回true
```

##### 解决方案

```
方案一:
	重写shou1dComponentupdate()方法
	比较新旧state或props数据，如果有变化才返回true，如果没有返回false
方案二:
	使用PureComponent
	PureComponent重写了shouldComponentupdate()，只有state或props数据有变化才返回		true
注意:
	只是进行state和props数据的浅比较，如果只是数据对象内部数据变了，返回false不要直接修		改state数据，而是要产生新数据
	项目中一般使用Pur eComponent来优化
```

#### 错误边界

```
理解：错误边界(Error boundary):用来捕获后代组件错误，渲染出备用页面
特点:
	只能捕获后代组件生命周期产生的错误，不能捕获自己组件产生的错误和其他组件在合成事件、定		时器中产生的错误
```

##### 使用方式

```
getDerivedStateFromError配合componentDidCatch

	//生命周期函数，一旦后台组件报错，就会触发
	static getDerivedstateFromError (error) {
		console.log(error );
		//在rende之前触发
		//返回新的state
		return {
			hasError : true,
		};
	}
	componentDidcatch(error ,info) {
	//统计页面的错误。发送请求发送到后台去
	console.log(error,info);
	}
```

