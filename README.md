前端开发笔记
-------
（主要是移动前端，包括自己遇到的坑）

####css相关
- 多行文本溢出显示“...”
``` css3
	display: -webkit-box;
	-webkit-line-clamp: 2;
	-webkit-box-orient: vertical;
	white-space: nawrap;
	overflow: hidden;
	text-overflow: ellipsis;
```

####浏览器特性差异
- `firefox`绝对定位不显式设置`left`值时的计算值并不是从父元素的左侧，而是父元素的可用空间的左侧
而`chrome`下面`left`计算值是`auto`。所以为了避免出现如图的问题，需要显示设置其`left`值为0；

####移动端踩坑
- 用`input`元素的`focus()`方法调起软键盘必须在用户的`click`事件下（浏览器处于安全考虑），在`tap`事件下键盘可能会出现闪退。

- **Android平台**
`Android`上的`firefox`在页面初始化的时候会将`window`的`width`置为默认宽度`980px`，等到页面完成时才会计算所需宽度并将`window`置为真正值，此时会默认触发在`window`上的`onload`和`onresize`事件。所以想要取得`window`的真正`width`值应该在这两个事件的回调方法里面写。

- **canvas rendered twice on Android device browser**
在`Android`设备上使用`canvas`绘图，有时候会出现画了两次的情况，解决方法是在`canvas`的包裹元素上的`overflow:hidden`改为`visible`

- **iOS平台**

- **flex相关**
在三星GT-N7100自带浏览器，opera， 搜狗，360，uc,猎豹，百度浏览器上，父元素即使设置了display为flex，子元素如果没有显示设置display的值，即使设置flex:1，这些浏览其还是将其以原有元素级别渲染。即：行内元素仍然渲染成行内元素，不会填充父元素剩余空间。

- **zepto相关**
	- **zepto点透问题**
		即使在回调里面用`event.preventDefault()`也没用，因为`zepto`中是用`setTimeout()`来包裹回调方法的，并不能马上执行。如下：
	``` javascript
	// delay by one tick so we can cancel the 'tap' event if 'scroll' fires
	// ('tap' fires before 'scroll')
	tapTimeout = setTimeout(function() {
			touchTimeout = setTimeout(function(){
			}, 250)
		}, 0)
	```
		可以在回调里面用 `setTimeout(func,200)` 来暂时解决这个问题。（后来经验证，好像并没有完全解决，建议使用fastclick.js来解决点透问题）