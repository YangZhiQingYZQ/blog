# 关于Promise

## 模拟实现Promise

Promise利用三大手段解决回调低于

1. 回调函数延迟绑定
2. 返回值穿透
3. 错误冒泡

```javascript
// 定义三种状态
const PENDING = 'PENDING', // 进行中
	  FULFILLED = 'FULFILLED', // 已成功
	  REJECTED = 'REJECTED'; // 已失败
	  
class Promise {
	constructor(exector){
		// 初始化状态
		this.status = PENDING;
		// 将成功、失败结果放在this上，便于then、catch访问
		this.value = undefined;
		this.reason = undefined;
		// 成功状回调函数队列
		this.onFulfilledCallbacks = [];
	}
}
```

