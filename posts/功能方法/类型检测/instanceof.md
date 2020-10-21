# 关于instanceof

## 存在的问题

此次操作符存在多个全局作用域（一个页面包含多个框架的）的情况下，因为原生的构造函数都在格子的window对象中，所以在另一个框架中定义的对象，在不同框架中返回不同（比如Array，SON）

### 解决办法

在任何值上调用Object原生的toString()方法，都会返回一个[Object NativeConstructoName]格式的字符传。每个类在内部都有一个[[Class]]属性，这个属性就指定了上述的构造函数名

```javascript
// 核心判断代码如下;
// 假设value值为一个数组对象
Object.prototype.toString.call(value);
// 执行此语句后返回[Object Array]

// 基于以上特性，可以创建判断值类型的函数
// 此函数接受两个参数，第一个参数是判断的值
// 第二个参数是期望的类型，限定为Array,Object,String,Number等
function IsValueType(val,type){
	const str = Object.prototype.toString.call(val),
		valType = str.slice(8,-1);
	return type == valType ;
}
```

由于原生数组的构造函数名与全局作用域无关，因此使用toString()就能保证返回一致的值；

#### 注意

* 对于IE中以COM对象实现的任何函数，Object.prototype.toString()都不会正常返回[object Fuction]，因为它们并不是原生的javascript函数
* 这种技巧是基于Object.prototype.toString()实现的，由于javascript是一种动态的语言，此函数本身可能会被修改，所以必须确保此函数是未被修改的原生版本；
