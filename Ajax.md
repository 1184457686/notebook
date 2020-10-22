## 什么是Ajax

```
ajax是一种用于创建快速动态页面的技术
通过在后台与服务器进行少量数据交换，AJAX 可以使网页实现异步更新。这意味着可以在不重新加载整个网页的情况下，对网页的某部分进行更新。
注：AJAX应用程序与浏览器和平台无关的！
```



## 工作原理

![image-20201015094323434](C:\Users\zhuyong\AppData\Roaming\Typora\typora-user-images\image-20201015094323434.png)

## 应用场景

> 1、页面上拉加载更多数据 2、列表数据无刷新分页 3、表单项离开焦点数据验证 4、搜索框提示文字下拉列表

## 请求参数格式

> application/x-www-form-urlencoded  适用格式   name=xxx&sex=xxx
>
> application/json  使用于json格式
>
> 注：get请求并不支持json数据格式   传统浏览器也不支持

## 状态码

>0 请求未初始化(未调用open())
>
>1 ：请求未发送     即 未调用send()
>
>2 请求以发送
>
>3  请求正在处理   部分响应数据已经可用
>
>4  响应以完成

### 获取ajax状态

```
xhr.readyState()
```

### 获取服务器的响应

``` 
onreadystatechange()  当状态变化时触发
```

## 错误处理

> 1、服务端收到请求，但结果不是预期结果  处理方案： 判断服务端返回的状态码  分别处理
>
> 2、服务端没有收到请求 返回404状态码   处理方案：检查请求地址是否有误
>
> 3、收到请求  返回500状态码  处理方案：调试后端代码
>
> 4、网络中断，请求无法发送到服务端  处理方案：在onerror事件函数中对错误进行处理

## Ajax在低版本IE浏览器的缓存问题

>问题:在低版本的IE浏览器中，Ajax请求有严重的缓存问题，即在请求地址不发生变化的情况下，只有第
>一次请求会真正发送到服务器端，后续的请求都会从浏览器的缓存中获取结果。即使服务器端的数据更新了，客户端依然拿到的是缓存中的I旧数据。
>
>> 解决方案：在请求地址的后面加请求参数，保证每一次请求中的请求参数的值不相同。
>>
>> ```
>> xhr.open( ' get', ' http:// www.example.com?t=' +Math.random ());
>> t 参数名称  
>> 注 参数名称不能和请求要求的参数名一样
>> ```

## Ajax封装

```
eg:
封装：function ajax(options){
		//请求默认参数
		var defaults={
			type:'get',
			url:'',
			data:{},
			header:{
				'Content-Type':'application/x-www-form-urlencoded'
			},
			success:function(){},
			error:function(){}
		}
		Object.assign(defaults,options) //该方法用于对象覆盖
		var xhr = new XMLHttpRequest()
		var params = '' 变量
		for(var attr in default.data){
			params+=attr+'='+default.data[attr]+'&'
		}
		params = params.substr(0,params.length-1)
		if(default.type=='get'){
			default.url =default.url+'?'+params
		}
		xhr.open(default.type,default.url)
		if(defalut.type=='post'){
			var contentType = default.header['Content-Type']
			xhr.setRequestHeader('Content-type':contentType)
			if(contentType=='application/json'){
				xhr.send(JSON.stringify(default.data))
			}else{
				xhr.send(params)
			}
		}else{
			xhr.send()
		}
	xhr.onload = function(){
        var contentType=xhr.getResponseHeader('Content-Type')
        var responseText =xhr.responseText;
        if(contentType.includes('application/json')){
            responseText = JSON.parse(responseText)
        }

        if(xhr.status ==200){
            defaults.success(responseText,xhr)
        }else{
            defaults.error(responseText,xhr)
        }
      	}
		
	}
```

## 模板引擎

> 作用 将数据和html拼接起来
>
> art-teamplat模板引擎

## FormData对象

### 作用

> 1、模拟html表单 相当于将HTML表单映射成表单对象，自动将表单对象中的数据拼接成请求参数的格式。
>
> 2、异步上传二进制文件  如图片等

注: 不能用于get请求

### FormData方法

> formData.get(key) 获取表单对象中的属性的值  		key表示表单中属性的名称 
>
> formData.set(key,value) 设置表单对象中的属性的值  		key表示表单中属性的名称
>
> > 如果key的值存在  将会替换   如不存在  将会创建
>
> formData.delete()  删除表单属性中的值
>
> formData.delete('key',value)  向表单追加属性中的值
>
> > set和append方法的区别：在属性名已存在的情况下 set会覆盖已有的键名  append会保留两个值

### FormData二进制文件上传

注：请求方式必须时post

```
form.keepExtensions=xxx  xxx=true时代表保留后缀名  反之则不保留
```

### FormData 文件上传进度展示

> xhr.upload.onprogress=function(){}
>
> eg: xhr.upload.onprogress = function (ev){
> 		//ev.loaded文件已经上传了多少
> 		//ev.total上传文件的总大小
> 		(ev.loaded / ev.total)* 100 +'%’;
> 	}

### FormaData 文件上传图片及时预览



### formidable工具

```
eg：
const formidable = require('formidable')
app.post('/formdata', (req, res) => {
    // 创建formidable表单解析对象
    const form = new formidable.IncomingForm();
    // 解析客户端传过来的FormData对象
    form.parse(req, (err, fields, files)=>{
        res.send(fields)
    })
})
```

> uploadDir  用于设置上传文件的存储路径    路径推荐适用绝对路径

### 同源政策

> 目的：保证用户信息的安全，防止恶意的网站窃取数据 
>
> ​	是浏览器给予ajax技术的限制   服务器端是不存在的同源政策

### AJax请求限制（JSONP) (CORS)

> ajax只能向自己的服务器发送请求 无法向非同源地址发送请求(请求能发送出去，但浏览器会拒绝接受)
>
> 解决方案：1、 JSONP(json with padding)  不适于ajax请求  但是可以模拟ajax请求  	注：需要前后端配合
>
> ​					 		1、将不同源的服务器端请求地址写在script标签的src属性中、
>
> ​					  		2、服务器端响应数据必须是一个函数的调用，真正要发送给客户端的数据需要作为函数调								用的参数。
>
> ​				     		3、在客户端全局作用域下定义函数fn 
>
> ​								4、在fn函数内部对服务器端返回的数据进行处理
>
> ​				2、CORS(Cross-origin resource sharing 即跨域资源共享) 允许浏览器向跨域服务器发送ajax请求
>
> ​							![image-20201020145411921](C:\Users\zhuyong\AppData\Roaming\Typora\typora-user-images\image-20201020145411921.png)
> 响应成功 响应头才会有 (Access-Control-Access-Origin)
>
> app.get( url, (req,res)=>{
> // 允许哪些客户端访问我
> 	 res.header('Access-Control-Allow-Origin','*')
> //代表允许所有的客户端访问我
> //允许客户端使用哪些请求方法访问我
> 	res.header('Access-Control-Allow-Methods','get,post')
>//是否允许请求时携带cookie
> res.header('Access-Control-Allow-Credentials')
> 	res.send('ok')
> });



> 如果两个页面拥有相同的协议、域名、端口  那么这两个网页就属于同一个源
>
> 注：实质是使用get请求

### JSONP代码优化

#### 客户端

> 客户端需要将函数名称传递到服务器端		
>
> 将script请求的发送变成动态请求(appendchild())
>
> 封装jsonp函数 方便请求发送

```
function jsonp(options){
	//动态创建script标签
	var script =document.createElement('script')
	//拼接字符串
	var param=''
	for(var attr in options.data){
		param+='&'+attr+'='+options.data[attr]
	}
	var fnName = 'MyJsonp'+Math.random().toString().replace(".","")
	//将success变成全局方法,并取名
	window[fnName] = options.success
	script.src=options.url+'?callback='+fnName+param
	//将script标签追加到body内
	document.body.appendChild(script)
	//为script添加onload事件
	script.onload=function(){
	document.body.removeChild(this)
	}
}
调用: eg  jsonp({
			url:'http://localhost:3001/better?callback=fn2'，
			data:{
				name:'li',
				age:12
			}
			success:function(){}
		})
```

#### 服务端

```
app.get(url,(req,res)=>{
	//接受传过来的函数名称
	//将函数名称对应的函数调用代码返回给客户端
	res.jsonp({name:"lisi",age}) 
})
```

#### 访问非同源服务器端解决方案

> 导入 request模块
>
> npm insatll request
>
> eg:request(url,(err,response,body)=>{})

#### withCredentials属性

> 在使用Ajax技术发送跨域请求时，默认情况下不会在请求中携带cookie信息。
>
> withCredentials:指定在涉及到跨域请求时，是否携带cookie信息，默认值为false
> Access-Control-Allow-Credentials: true 允许客户端发送请求时携带cookie
#### session

> 下载session模板  npm install express-session -d -save
>
> 引入session模板  const session = require('express-session')
>
> 初始值设定：
>
>  app.use(session({
>
>   secret: "weird sheep",
>
>   resave: false,
>
>   saveUninitialized: true,
>
>   cookie: { user: "default", maxAge: 14 * 24 * 60 * 60 * 1000 }
>
> }));


## $.ajax()

```
eg:
$.ajax({
	type: 'get ' ,
	url: 'http :// www.example.com' ,
	data: { name: ' zhangsan' , age : '20'},
	contentType: 'application/z-www一form-urlencoded' ,
	beforesend : function (){
	return xxx
  },
	success:function (response){ }，
	error: function (xhr){ }
});
```

> 作用:发送ajax请求
>
> data：无论是对象还是拼接好的形式  $.ajax内部会默认以参数字符串的形式发送给服务器
>
> contentType   设置参数格式
>
> beforesend 请求之前会被调用   如果return false则会终止请求
>
> $ .ajax()根据服务器返回的类型自动将数据转换成相应的类型 如：服务器返回json字符串 ，$.ajax会自动将返回的数据转换成json对象

###  serialize方法

> 作用：将表单中的数据自动拼接成字符串类型的参数
>
> eg: var params = $('#form' ).serialize () ;
> 		//name=zhangsan&age=30这种样式

```
封装：
function serializeObject(obj){
 	var result={}
 	// serializeArray() 将数据转换成数组类型
 	var params =obj.serializeArray()
 	$.each(params,function(index,value){
 		result[value.name] = value.value
 	})
 	return result;
 }
```

### $.ajax发送jsonp请求

> 在$.ajax()中添加一个 dataType  
>
> > 可选参数 jsonop 修改callbak参数名称 
> >
> > ​				jsonCallback:'xxx' 指定函数名称
>
> ​	

```
eg:
	eg:
$.ajax({
	type: 'get ' ,
	url: 'xxx' ,
	dataType:'jsonp'
	data: { name: ' zhangsan' , age : '20'},
	contentType: 'application/z-www一form-urlencoded' ,
	beforesend : function (){
	return xxx
  },
	success:function (response){ }，
	error: function (xhr){ }
});
```

### $.get()  $.post()

> $.method(url,data,callback) 
>
> ​	data可以是对象也可以是字符串  可选     具体看服务端需不需要

### 全局事件

> .ajaxStart() //请求开始发送时触发
>
> .ajaxComplete()  //请求完成时触发	

> 插件  NProgress  进度条插件 
>
> > NProgress.start()  NProgress.done() 

## XML

> XML的全称是extensible markup language，代表可扩展标记语言，它的作用是传输和存储数据。

> XMLDOM即XML文档对象模型，是w3c组织定义的一套操作XML文档对象的API。浏览器会将XML文档解析成文档对象模型。

