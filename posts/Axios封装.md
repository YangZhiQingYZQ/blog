<!--
 * @Author: YZQ
 * @Description: axios封装
 * @Date: 2020-10-13 14:33:54
 * @LastEditors: YZQ
 * @LastEditTime: 2020-10-13 16:45:11
-->

# Axios 封装

## 介绍

主要用于 vue 中

## 安装

```
npm install -d axios 或 yarn add axios
```

## 引入

```javascript
import aixos from "axios"; // axios包
import GetApi from "./api"; // 封装的api模块
import { Message } from "element-ui"; // 引入element-ui的message组件，用作公共提示
import router from "@/router"; // vue路由
import store from "@/store"; // vue状态管理仓库
```

## 说明
![项目文件结构](https://github.com/YangZhiQingYZQ/blog/posts/image/Axios封装/0.png)


## 封装思路

- 不同环境的域名配置
- 公共的请求配置
- 发送请求前/后需要进行的一些统一处理

### 不同环境的域名配置

通过配置 process.env.NODE_ENV 在不同环境的值来判断使用的域名

```javascript
if (process.env.NODE_ENV == "development") {
  const baseURL = "https://www.baidu.com";
} else if (process.env.NODE_ENV == "debug") {
  const baseURL = "https://www.ceshi.com";
} else if (process.env.NODE_ENV == "production") {
  const baseURL = "https://www.production.com";
}
```

### 公共的请求配置

设置默认的请求头

```javascript
const headers = {
  Accept: "application/json",
  Authorization: "",
};
```

### 创建自己的 axios 实例

```javascript
const axiosConfig = {
  baseUrl: baseUrl,
  headers: headers,
};
const myAxios = axios.create(axiosConfig);
```

### 设置拦截器

请求拦截器

```javascript
myAxios.interceptors.request.use((config) => {
  // GetApi是通过api模块中抛出的方法，通过传入的urlKey进行查找
  config.url = GetApi(config.url);
  const token = store.state.token;
  token && (config.headers.Authorization = token);
  return config;
});
```

响应拦截器

```javascript
// 根据后端返回的数据格式进行处理
// 这里假设后端与后端约定的数据格式为：
// res = {
//      msg:string, 错误信息
//      status:number, 错误码,正常为2000,
//      data:any 页面数据
// }
myAxios.interceptors.response.use(
  (res) => {
    if (res.data.status !== 2000) {
      const { status } = res.data;
      // 所有接口公共的错误处理
      switch (status) {
        // 未登录
        case 401:
          store.dispatch("ToLogin");
          break;
        // 登录已过期
        case 403:
          store.dispatch("LoginDate");
          break;
        default:
          // 其他异常情况
          console.log(res);
      }
      // 个别接口错误时，在请求的地方进行特殊处理
      return Promise.reject(res);
    }
    return Promise.resolve(res.data);
  },
  (err) => {
    // 可以做一些处理
    err.status == 500 && Message.error("服务器异常，请求稍后再试！");
  }
);
```
##  实际使用
### 在vue的main.js中将自定义的axios挂载到原型上
```javascript
// main.js
// 其他引入
import myAxios from '@/request'
vue.prototype.$axios = myAxios;
// 其他代码....
new Vue({
  el: '#app',
  router,
  store,
  render: h => h(App)
})
```

### 在页面中调用
```javascript
methods:{
    testAjax(){
        this.$axios.post('LOGIN',params).then(res=>{
            // 正常错误业务逻辑
        },err=>{
            // 这里做公共错误之外的特殊错误处理
        })
    }
}
```
