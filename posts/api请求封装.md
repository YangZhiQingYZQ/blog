# Api 封装

## 介绍

## 封装思路

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
