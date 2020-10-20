# Vue项目优化

## 代码层面优化

* v-if和v-show区分使用场景
* computed和watch区分使用场景
* v-for遍历必须为item添加key，且避免同时使用v-if
* 长列表性能优化
* 事件的销毁
* 图片资源懒加载
* 路由懒加载
* 第三方插件的按需引入
* 优化无限列表性能
* 服务端渲染 SSR or 预渲染

## Webpack层面优化

* Webpack对图片进行压缩
* 减少ES6转为ES5的冗余代码
* 提取公共代码
* 模板预编译
* 提取组件的CSS
* 优化SourceMap
* 构建结果输出分析
* Vue项目的编译优化

## 基础的Web技术的优化

* 开启gzip压缩
* 浏览器缓存
* CDN的使用
* 使用Chrome Performance查找性能瓶颈