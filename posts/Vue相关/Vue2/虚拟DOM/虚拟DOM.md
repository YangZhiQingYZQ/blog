# 虚拟DOM

## 定义

virtual DOM就是对DOM的抽象，本质上是JavaScript对象，这个对象就是更加轻量级的对DOM的描述

## 什么需要Virtual DOM

因为操作DOM相对较慢，并且因为频繁变动DOM会造成浏览器的回流或者重绘，十分影响性能

现代前端框架的一个基本要求就是无需手动操作DOM，一方面是因为手动操作DOM无法保证程序性能，多人协作开的项目中如果review不严格，可能会有开发者写出性能较低的代码，另一个方面更重要的是省略手动DOM操作可以大大提高开发效率

而且Virtual DOM最初的目的就是更好的跨平台，而服务端中并不存在DOM，所以Node.js通过Virtual实现SSR（服务端渲染）就成了可能

## 虚拟DOM的优缺点

### 优点：

* 保证性能下限：框架的虚拟DOM所需要适配任何上层API可能产生的操作，它的一些DOM操作的实现必须是普适的，所有它的性能并不是最优的；但是比起粗暴的DOM操作性能要好很多，因此框架的虚拟DOM至少可以保证你不需要手动优化的情况下，依然可以提供还不错的性能，即保证性能的下限；
* 无需手动操作DOM：我们不再需要手动去操作DOM，只需要写好View-Model的代码逻辑，框架会根据虚拟DOM和数据双向绑定，帮我们以可预期的方式更新视图，极大提高我们的开发效率
* 跨平台：虚拟DOM本质上是JavaScript对象，而DOM这与平台强相关，相比之下虚拟DOM可以进行更方便地跨平台操作，例如服务器渲染、weex开发等等

### 缺点

* 无法进行极致优化：虽然虚拟DOM + 合理的优化，足以应对绝大部分应用的性能需求，但在一些性能要求极高的应用中虚拟DOM无法进行针对想的极致优化

## 虚拟DOM的实现原理

主要包括以下3个部分

* 用JavaScript对象模拟真实DOM树，对真实DOM进行抽象
* diff算法 —— 比较两颗虚拟DOM树的差异
* pach算法 —— 将两个虚拟DOM对象的差异应用到真正的DOM树

## 将VirtualDom转化为真实DOM结构

这个是SPA应用的核心概念之一

```javascript
// vnode结构
// {
//		tag,
//		attrs,
//		children,
//		...
//		}

// Virtual DOM => DOM
function render(vnode,container){
	container.appendChild(_render(vnode));
}
function _render(vnode){
	// 如果是数字类型转化为字符串
	if(typeof vnode === 'number'){
		vnode = String(vnode);
	}
	// 字符串类型直接就是文本节点
	if(typeof vnode === 'string'){
		return document.createTextNode(vnode);
	}
	// 普通DOM
	const dom = document.createElement(vnode.tag);
	if(vnode.attrs){
		// 遍历属性
		Object.keys(vnode.attrs).forEach(key => {
			const value = vnode.attrs[key];
			dom.setAttribute(key,value);
		})
	}
	// 子数组进行递归操作
	vnode.children.forEach(child => render(child,dom));
	return dom;
}
```