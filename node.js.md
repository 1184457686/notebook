##  `EventEmitter`

```
events模块 只提供一个对象EventEmitter 通过require("events") eg：var event = require("events")
EventEmitter(事件发送器)  核心就是事件触发与事件监听器功能的封装。
创建EventEmitter类 eg：var eventEmitter = new events.EventEmitter();
```

#### `EventEmitter`方法

```
addListener(event, listener)为指定事件添加一个监听器到监听器数组的尾部
on(event, listener) 为指定事件注册一个监听器，接受一个字符串 event 和一个回调函数
once(event, listener)为指定事件注册一个单次监听器，即 监听器最多只会触发一次，触发后立刻解除该监听器
removeListener(event, listener)移除指定事件的某个监听器，监听器必须是该事件已经注册过的监听器。它接受两个参数，第一个是事件名称，第二个是回调函数名称。
emit()  按顺序执行每个监听器，如果事件有注册监听返回 true，否则返回 false
```

#### error事件

```
EventEmitter 定义了一个特殊的事件 error，它包含了错误的语义，我们在遇到 异常的时候通常会触发 error 事件。当 error 被触发时，EventEmitter 规定如果没有响 应的监听器，Node.js 会把它当作异常，退出程序并输出错误信息。我们一般要为会触发 error 事件的对象设置监听器，避免遇到错误后整个程序崩溃。
```

注意：一般不会直接使用`EventEmitter`  而是采用在对象中继承它

##  Buffer(缓冲区)





```
目前支持的编码格式：
ascii - 仅支持 7 位 ASCII 数据。如果设置去掉高位的话，这种编码是非常快的。
utf8 - 多字节编码的 Unicode 字符。许多网页和其他文档格式都使用 UTF-8 。默认编码格式
utf16le - 2 或 4 个字节，小字节序编码的 Unicode 字符。支持代理对（U+10000 至 U+10FFFF）
ucs2 - utf16le 的别名。
base64 - Base64 编码。
latin1 - 一种把 Buffer 编码成一字节编码的字符串的方式。
binary - latin1 的别名。
hex - 将每个字节编码为两个十六进制字符。
```

### 写入缓冲区

```
语法: buf.write(string[,offset][,encoding]) 返回值: 实际写入的大小
string:写入缓冲区的字符串
offset:缓冲区开始写入的索引值 默认为0
encoding 使用的编码  默认为utf-8

eg:var buf = Buffer.alloc(256,1,'utf8')
```

### 读取缓冲区

```
语法：buf.toString([encoding[,start[,end]]]) 返回值:解码缓冲区数据并返回指定字符串
strat:开始读取的索引位置 默认为0
end:结束位置（不包含end位置的值）
```

### 缓冲区合并

```
语法:Buffer.concat(list[,totalength]) 返回值：一个合并后的Buffer对象
list：指定合并的Buffer对象数组列表
totallength:指定合并后的长度
```

### 拷贝缓冲区

```
语法：buf.copy(targetBuffer[, targetStart[, sourceStart[, sourceEnd]]]) 没有返回值
targetBuffer:要拷贝的BUffer对象
```

### 缓冲区裁剪

```
语法：buf.slice([start[,end]]) 返回值:返回一个新的缓冲区，它和旧缓冲区指向同一块内存，但是从索引 start 到 end 的位置剪切。
```

### 方法

```
buf.length  返回这个 buffer 的 bytes 数。注意这未必是 buffer 里面内容的大小，length 是 buffer 对象所分配的内存数，它不会随着这个 buffer 对象内容的改变而改变。
```

补充:Buffer底层存储时采用二进制 ，显示时为了方便采用的是16进制

##  Stream(流)

### 流类型

```
Readable-可读操作   Writable--可写操作  Duplex--可读可写操作   transform----操作被写入数据，然后读出结果
```

注意：所有的Stream对象的都是EventEmitter的实例

```
常用事件：data-数据可读时触发 end--没有更多数据可读时触发  error--接受和写入时发生错误时触发 
finish - 所有数据已被写入到底层系统时触发
```

#### 读取流

```
语法：fs.Readerable('文件')
```

#### 写入流

```
语法：fs.write(data,encoding)  encoding代表编码格式
```

#### 管道流(提供了一个输出流到输入流的机制)

```
语法：fs.Readerable('文件') fs.write(data,encoding)
```

#### 链式流（连接输出流到另外一个流并创建多个流操作链的机制）

```
zlib模块 用于压缩和解压
```

补:链式流一般用于管道操作。

## 模块系统

```
创建模块:require()
Node.js 提供了 exports 和 require 两个对象，其中 exports 是模块公开的接口，require 用于从外部获取一个模块的接口，即所获取模块的 exports 对象。
```

注：exports 和 module.exports 的使用如果要对外暴露属性或方法，就用 **exports** 就行，要暴露对象(类似class，包含了很多属性和方法)，就用 **module.exports**。

## 常用工具

```
util.callbackify(fn) 将普通方法fn转换成(err,res)=>{}形式
util.inherits(constructor，superConstructor)是一个实现对象间原型继承的函数。
util.inspect(obj,boolean,depth,color) 是一个将任意对象转换 为字符串的方法  
	boolean:表示是否输出隐藏信息   true 表示输出
	depth:表示最大递归的层数 默认为2  指定为null  表示将不限递归层数完整遍历对象
	colors:boolean类型 输出格式将会以 ANSI 颜色编码显示  
注:util.inspect并不是单纯的将对象转换成字符串
util.isArray(objk) 判断obj是不是数组(是 返回true) 已经被淘汰  建议是用Array.isArray(obj)
util.isRegExp(obj) 判断obj是不是正则表达式   已经被淘汰  建议使用util.types.isRegExp(obj)
utile.isDate(obj) 判断obj是不是一个日期  建议使用util.types.isDate(obj)
```

 更多请见   [http://nodejs.org/api/util.html](https://nodejs.org/api/util.html) 

## 文件系统

```
模块: var fs = require('fs')
```

### 同步和异步

```
同步写法:fs.readFileSync()
异步写法：fs.readFile() 最后一个参数为回调函数，回调函数的第一个参数包含了错误信息(error)。
建议使用异步方法 异步方法性能更高，速度更快，而且没有阻塞。
```

### 打开文件

```
异步打开:fs.open(path, flags[, mode], callback)
	path - 文件的路径。
	flags - 文件打开的行为。具体值详见下文。
	mode - 设置文件模式(权限)，文件创建默认权限为 0666(可读，可写)。
	callback - 回调函数，带有两个参数如：callback(err, fd)。 fd:文件的描述符
注：当打开的文件使用完毕后要记得关闭文件
```

#### 常用mode

| Flag | 描述                                                   |
| ---- | :----------------------------------------------------- |
| r    | 以读取模式打开文件。如果文件不存在抛出异常。           |
| r+   | 以读写模式打开文件。如果文件不存在抛出异常。           |
| rs   | 以同步的方式读取和写入文件。                           |
| rs+  | 以同步的方式读取和写入文件。                           |
| w    | 以写入模式打开文件，如果文件不存在则创建。             |
| wx   | 类似 'w'，但是如果文件路径存在，则文件写入失败         |
| w+   | 以读写模式打开文件，如果文件不存在则创建。             |
| wx+  | 类似 'w+'， 但是如果文件路径存在，则文件读写失败。     |
| a    | 以追加模式打开文件，如果文件不存在则创建               |
| ax   | 类似 'a'， 但是如果文件路径存在，则文件追加失败。      |
| a+   | 以读取追加模式打开文件，如果文件不存在则创建。         |
| ax+  | 类似 'a+'， 但是如果文件路径存在，则文件读取追加失败。 |

### 获取文件信息

```
fs.stat(path, callback)
	path:文件路径
	callback:回调函数 带有两个参数如：(err, stats), stats 是 fs.Stats 对象
```

### 写入文件

```
fs.writeFile(file,data[,option],callback) options - 该参数是一个对象，包含 {encoding, mode, flag}。默认编码为 utf8, 模式为 0666 ， flag 为 'w'
	常用option：
	r：只读
	w:可写 会覆盖旧内容
	a:追加
eg:fs.writeFile(file,data,{flag:"a"},callback)
```

### 关闭文件

```
fs.close(fd,callback) 
fd -- 通过 fs.open() 方法返回的文件描述符。
callback 无参数
```

### 读取文件

```
fs.read(fd,buffer,offset,length,length,position,callback)
	fd:fs.open()返回的文件描述符
	buffer:数据写入的缓冲区
	offset:偏移量 (距离初始位置的距离)
	length:要从文件中读取的字节数
	position:文件开始读取的位置
```

补充：文件描述符作用：在系统层，所有文件系统操作都使用这些文件描述符来标识和跟踪每个特定的文件。

### 截取文件

```
fs.ftruncate(fd,len,callback)
 fd -- 文件描述符
 len 文件内容截取长度
 callback --回调函数  没有参数
```

### 创建目录

```
fs.mkdir(path[,option],callback)
	option参数如下
		recursive  是否以递归的方式创建目录
		mode:设置目录权限  默认为0777
	callback:回调函数   没有参数
```

### 读取目录

```
fs.readdir(path,callback)
	callback:回调函数  有两个参数  err:表示错误信息   files:代表path目录下的文件组列表表
```

### 删除文件

```
fs.ublink(path,callback)
	path:目标文件路径
	callback:回调函数
```

## GET/POST请求

### GET方法

```
特点：GET请求直接被嵌入在路径中，URL是完整的请求路径，包括了?后面的部分，因此你可以手动解析后面的内容作为GET请求的参数
	GET请求出现乱码现象解决方案：
	指定编码  eg: res.writeHead(200, {'Content-Type': 'text/plain;charset=utf-8'});
```

### POST方法

```
特点：POST 请求的内容全部的都在请求体中，http.ServerRequest 并没有一个属性内容为请求体，原因是等待请求体传输可能是一件耗时的工作。
```

### API

```
assert(断言)模块 通常用来对代码进行校验，若出错则阻止程序运行，并抛出一个错误。

```

## Express框架

```
创建项目:express+项目名
```

```
基础语法:
1、引入express模块  var express = require('express')
2、创建express实例  var app = express()
3、定义路由  app.method(path,function) 
	method：请求方式 get post put delete  小写
	path:服务器上的路径
4、 如果要使用静态文件  如图像 css js等 请使用express.staticExpress中的内置中间件功能
	express.static(root,[option])
	该root参数指定要从其提供静态资产的根目录
```

### 常见问题

#### 如何处理404

> 在Express中，404响应不是错误的结果，因此错误处理程序中间件将不会捕获它们。此行为是因为404响应仅表明没有要做的其他工作。换句话说，Express已经执行了所有中间件功能和路由，但发现它们均未响应
>
> > ```javascript
> > app.use(function (req, res, next) {
> >   res.status(404).send("Sorry can't find that!")
> > })
> > ```

#### 如何呈现纯html

> 无需使用该`res.render()`函数“渲染” HTML 。如果您有特定文件，请使用该`res.sendFile()`功能。如果要从目录提供许多资产，请使用`express.static()` 中间件功能。

### 连接数据库(mongodb）

>1、node.js直接操作mongodb  即原生node操作数据库
>
>2、基于mongoose插件操作数据库

```
语法：var mongoose = require('mongoose')
	 mongoose.connect('path',{useNewUrlParser:true,useUnifiedTopology:true})
	  path:路径  eg:mongodb://localhost:27017/test 
	  {userNewUrlOarser:true,uuseUnifiedTopology:true} 用于解析地址(新版本才加)
```

