# 作用域

  作用域是指程序源代码中定义变量的区域。

  作用域规定了如何查找变量，也就是确定当前执行代码对变量的访问权限。

  JavaScript 采用词法作用域(lexical scoping)，也就是静态作用域。
  
  **作用域类型**
    - 静态作用域   
      函数的作用域在函数定义的时候就决定了
    - 动态作用域    
      函数的作用域是在函数调用的时候才决定    
      [JavaScript深入之作用域链](https://github.com/mqyqingfeng/Blog/issues/3)的举例
      
      ```javascript
          var value = 1;
          
          function foo() {
              console.log(value);
          }

          function bar() {
              var value = 2;
              foo();
          }

          bar();
          
          // 结果是?
      ```
# 作用域链

当查找变量的时候，会先从当前上下文的变量对象中查找，如果没有找到，就会从父级(词法层面上的父级)执行上下文的变量对象中查找，一直找到全局上下文的变量对象，也就是全局对象。这样由多个执行上下文的变量对象构成的链表就叫做作用域链。



