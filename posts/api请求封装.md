# Api 封装

## 需求背景

在项目中除了静态页面的项目之外几乎都无法避免的一个工作就是与后端对接api了；

在项目很小的时候，可以将请求路径直接写在使用的请求函数中，因为即使需要进行维护成本也不高。但是随着项目的增大，api的数量增加，人员的增加，导致维护成本越来越高，此时api的统一管理就十分有必要了；

## 封装思路

* 使用模块化的方式分离不同功能模块的api
* 使用对象属性的方式来存储api
* 暴露出获取api的函数

## 实际代码

模块 login.js

```javascript
export default {
  LOGIN: "login",
  LOGIN_OUT: "loginOut",
};
```

模块 log.js

```javascript
export default {
  LOG_ERR: "log/err",
  LOG_WARN: "log/warn",
};
```

主模块 index.js

```javascript
import Login from "./login.js";
import Log from "./log.js";
const api = {
  ...Login,
  ...Log,
};
// 所有接口的地址前缀
const apiPrefix = '/api/'
const GetAPi(apiKey){
    return apiPrefix + api[apiKey]
}
export default GetApi;
```
