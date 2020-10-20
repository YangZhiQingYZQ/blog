
# Vue2与Vue3对比

## Proxy与Object.defineProperty优劣对比

### Proxy的优势

* 可以直接监听对象而非属性
* 可以直接监听数据的变化
* 有13中拦截方法，不限于apply、ownKeys、deleteProperty、has等等是Object.defineProperty不具备的
* 返回的是一个新对象，我们可以只操作新的对象达到目的，而Object.defineProperty只能遍历对象属性直接修改
* 作为新标准将受到浏览器厂商重点持续性能优化，也就是新标准的性能红利

### Object.defineProperty的优势

* 兼容性好，支持IE9，，而Proxy存在浏览器兼容性问题，而且无法用polyfill磨平，因此Vue的作者才生命需要等到下个大版本（3.0）才能用Proxy重写