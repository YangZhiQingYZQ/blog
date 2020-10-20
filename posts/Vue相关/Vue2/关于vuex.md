## 关于vuex

以下模块：

* State：定义了应用状态的数据结构，可以在这里设置默认的初始状态
* Getter：允许组件从Store中获取数据，mapGetters辅助函数仅仅是将store中的getter映射到局部计算属性
* Muntation：唯一更改store中状态的方法，且必须是同步函数
* Action：用于提交mutation，而不是直接变更状态，可以包含任意异步操作
* Module：允许将单一的Store拆分为多个store且同时保存在单一的状态树中