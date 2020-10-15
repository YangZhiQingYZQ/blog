# JavaScript的全局属性/函数

## 介绍

JavaScript的全局对象是Global对象，所有不属于任何其他对象的属性和方法，都是Global对象的属性和方法，所有在全局作用域定义的属性和方法都是定义在Global对象上的，但无法访问到Global对象；

注意，window并不等同于Global对象，window对象是由浏览器厂商实现的，在浏览器环境中window会将Global对象的属性与方法当作window对象的一部分来实现；

## 全局属性

* Infinity：代表正的无穷大的数值
* NaN：只是某个值不是数字值
* undefined：指示未定义的值

## 全局函数

* decodeURI：解码某个编码的URI
* decodeURIComponent：解码一个编码的URI组件
* encodeURI：把字符串编码为URI
* encodeURIComponent：把字符串编码为URI组件
* escape：对字符串进行编码
* eval：计算JavaScript字符串，并把它作为脚本代码来执行
* isFinite：检查某个值是否为有穷大的数
* isNaN：检查某个值是否是数字
* Number：把对象的值转换为数字
* parseFloat：解析一个字符串并返回一个浮点数
* parseInt：解析一个字符串并返回一个整数
* String：把对象的值转换为字符串
* unescape：对由escape编码的字符串进行解码
还包括所有原生引用类型的构造函数，如：Date，Boolean等
## 注意

JavaScript中只有global全局对象，并没有window全局对象，window对象是浏览器厂商实现的，所以window对象下的方法与属性不能算作全局函数

## 参考

菜鸟教程：[JavaScript 全局属性/函数](https://www.runoob.com/jsref/jsref-obj-global.html)
JavaScript高级程序设计
