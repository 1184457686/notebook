# 基础理论

### 查看版本

```jQuery
$.fn.jquery
```
## 语法
> `jQuery` 语法是通过选取 HTML 元素，并对选取的元素执行某些操作。

```
 $(selector).action()
 	seletor 选择符  
 	action:对选择的元素的操作
 eg:$('#test').click(function(){})
action常用事件:
	ready:防止文档在完全加载（就绪）之前运行 jQuery 代码，即在 DOM 加载完成后才可以对 DOM 进行操作。
	click:点击事件
	dblclick:双击事件 注意 当一个元素同时绑定了click事件和dblclick事件  双击时也会触发click事件
	mouseenter:鼠标穿过即当鼠标从元素中间穿过触发
	mouseleave:鼠标离开元素时触发
	mouseup:松开鼠标按键时触发
	hover:鼠标悬停
	focus:聚焦
	blur:失焦
```

```
鼠标事件中MouseDown、MouseUp与Click事件有什么区别？
Mouse Down是鼠标按下触发的动作；Mouse Up是鼠标抬起触发的动作；Mouse  Click就是按下又抬起的动作；click是激活,包含了MouseClick,MouseClick是鼠标点击；
click不只是鼠标点击,当焦点在该控件上,按回车时也激发此事件，MouseClick应该有鼠标点击坐标属性成员。
```

## 效果

```
hide() 隐藏 实质就是给元素添加display:none
show() 显示  仅用于hide()或css中display:none隐藏的元素
fadeIn() 淡入
fadeOut()淡出
fadeToggle() 自动在淡入和淡出中切换
fadeTo 设置渐变透明度 没有默认参数，必须加上  slow/fast/Time  
slide 滑动
animate() 动画
stop() 停止动画 仅停止当前动画 stopAll设置为true可停止全部动画 eg $('p').stop(true)即可停止p标签的		 所有动画
方法链 . eg $("#p1").css("color","red").slideUp(2000).slideDown(2000);
			"p1" 元素首先会变为红色，然后向上滑动，再然后向下滑动
```

## Html

```
获取内容 
	text() 设置或获取文本内容  html() 设置或获取内容 包括html标签 val 设置或获取表单字段的值 attr 	
	获取属性值
设置 同上 以上方法都可以拥有回调函数  回调函数有两参数i,orign
		i 被选元素列表中当前元素的下标
		orign 原始的值
	attr 允许同时设置多个值
添加元素  append() 选元素的结尾插入内容
		prepend() 在被选元素的开头插入内容
		after,before 添加内容
总结：
	append/prepend 是在选择元素内部嵌入。
	after/before 是在元素外面追加。
	删除 remove() 删除元素及其子元素 empty()删除子元素
尺寸 设置了 box-sizing 后，width() 获取的是 css 设置的 width 减去 padding 和 border 的值
```

# Jquery Ajax

## 方法

```
load()方法 用于从服务器加载数据，并把返回的数据放入所选元素中
语法 $(selector).load(URL,data,callback);
	url:需要加载的url
	data 规定与请求一同发送的查询字符串键/值对集合。可选
	callback(responseTxt,statusTxt,xhr) 回调函数
		responseTxt 调用成功时的结果内容
		statusTxt 调用时的状态
		xhr 包含 XMLHttpRequest 对象
```

## get/post

![image-20201014163535191](C:\Users\zhuyong\AppData\Roaming\Typora\typora-user-images\image-20201014163535191.png)

# 其他

```
noConflict() 用于释放$符号 防止如果在用的两种不同的框架正在使用相同的简写符号，有可能导致脚本停止运行
```

