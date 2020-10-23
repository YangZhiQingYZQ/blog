# 关于instanceof

用于检测构造函数的prototype属性是否出现在某个实例对象的原型链上

## 存在的问题

使用instanceof进行判断时，只能证明当时判断的结果，因为javascript是一种动态的语言，所以检测对象的原型链有可能因为有意或无意的赋值导致原型链被更改；更改的场景如下：

* 构造函数的原型对象更改后，通过此构造函数创建出来的对象就有可能导致之前的判断错误
* 虽然目前的ES规范中，我们只能读取对象的原型而不能改变它，但借助非标准的__proto__伪属性是可以更改对象的原型的

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
* 此方法只能判断此值是否属于某个原生类型，无法判断是否由哪个构造函数创建


## 模拟instanceof 

instanceof运算符用于检测构造函数的prototype属性是否出现在某个实例对象的原型链上

```javascript
const myInstanceof = (left,right){
	// 基本类型都返回false
	if(typeof left !== 'object' || left == null) return false;
	let proto = Object.getPrototypeOf(left);
	while(true){
		if(proto === null) return false;
		if(proto == right.prototype) return true;
		proto = Object.getPrototypeOf(proto);
	}

}
```