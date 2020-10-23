# 关于typeof

typeof操作符返回一个字符串，表示未经过计算的操作数类型

## 语法

```javascript
// value为操作数：表示一个对象或原始值的表达式，此语句返回此操作数类型
typeof value
```

## 可能返回的值

| 类型 | 结果|
|Undefined | "undefinde"|
|Null | "object"|
| Boolean | "boolean"|
| Number |"number"|
|BigInt(ECMAScript 2020新增)| "bigint"|
| String | "string"|
|Symbol | (ECMAScript 2015新增)|
|宿主对象（由JS环境提供）| 取决于具体实现|
| Function对象（按照ECMA-262规范实现[[Call]]）| "function"|
| 其他任何对象 | "object"|

## 问题

* 使用typeof判断null类型返回 object，此方法是javaScript的设计缺陷，但因为各种原因无法进行更正