# 生命周期
  ![avatar]https://cn.vuejs.org/images/lifecycle.png
- beforeCreate
  
- created
  当前阶段完成数据观测，可以进行初始的数据获取，也可以更改数据，在这里更改数据不会触发updated函数
- beforeMount
  template模板导入渲染函数编译，虚拟dom已经创建完成，即将开始渲染。
- mounted
  双向数据绑定完成，dom挂载完成，可以获取dom节点，使用$refs对Dom进行操作
- beforeUpdate
  响应式数据发生变化，虚拟dom重新渲染之前，可以在当前阶段修改数据，不会造成重渲染
- updated
  数据更新完成，组件dom完成更新，在此阶段避免修改数据，以防造成无限循环的更新。
- beforeDestory
  在实例被销毁之前，可以进行善后工作，比如清除定时器。
- destoryed
  实例被销毁之后，只剩下dom空壳。
