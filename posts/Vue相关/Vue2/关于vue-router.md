# 关于vue-router

## vue-router 路由模式有几种？

vue-router有3中路由模式：hash、history、abstract。对应源码如下所示：

```javascript
switch(mode){
	case 'history':
		this.history = new HTML5History(this,options.base);
		break;
	case 'hash':
		this.history = new HashHistory(this,options.base,this.fallback);
		break;
	case 'abstract':
		this.history = new AbstractHistory(this,options.base);
		break;
	default:
		if(process.env.NODE_ENV !== 'production'){
		 assert(false,`invalid mode:${mode}`);
		}
}
```
其中模式说明如下：

* hash：使用URL hash值作为路由。支持所有浏览器，包括不支持HTML5 History Api的浏览器
* history：依赖HTML5 History API 和服务器配置
* abstract：支持所有JavaScript运行环境，如Node.js服务端。如果发现没有浏览器的API，路由会自动强制进入这个模式

## Vue-router中的hash和history路由模式实现原理

### hash 模式实现原理

早期的前端路由的实现就是基于location.hash来实现的。实现原理就是根据location.hash值就是URL中#后的内容

hash路由模式的实现主要基于下面的几个特性

* URL中hash值只是客户端的一种状态，也就是说当向服务端发送请求时，hash部分不会被发送
* hash值的改变，都会在浏览器的访问历史中增加一个记录。因此我们能通过浏览器的回退、前进按钮控制hash的切换；
* 可以通过a标签，并设置href属性，但用户点击这个标签后，URL的hash值会发生改变；或者使用JavaScript来对location进行赋值，改变URL的hash值；
* 我们可以使用hashchange事件来监听hash值得变化，从而对页面进行跳转（渲染）

### history 模式实现原理

HTML5提供了History API来实现URL的变化。其中做最主要的API有以下两个：history.pushState()和history.repalceState()。这两个API可以在不进行刷新的情况下，操作浏览器的历史记录。唯一不同的是，前者是新增一个历史记录，后者是直接替换当前的历史记录，如下所示：

```javascript
window.history.pushState(null,null,path);
window.history.replaceState(null,null,path);
```
history路由模式的实现主要基于存在下面几个特性：

* pushState和replaceState两个API来操作实现URL的变化；
* 我们可以使用popstate事件来监听url的变化，从而对页面进行跳转（渲染）
* history.pushState()或者history.replaceState()不会触发popstate事件，这时我们需要手动触发页面跳转（渲染）