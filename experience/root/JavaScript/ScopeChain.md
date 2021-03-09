# 作用域

  > 资料来自[JavaScript深入之词法作用域和动态作用域](https://github.com/mqyqingfeng/Blog/issues/3)

  作用域是指程序源代码中定义变量的区域。

  作用域规定了如何查找变量，也就是确定当前执行代码对变量的访问权限。

  JavaScript 采用词法作用域(lexical scoping)，也就是静态作用域。
  
  **作用域类型**    
  - 静态作用域   
    函数的作用域在函数定义的时候就决定了
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
  - 动态作用域    
    函数的作用域是在函数调用的时候才决定    
    
    ```javascript
        value=1
        function foo () {
            echo $value;
        }
        function bar () {
            local value=2;
            foo;
        }
        bar
    ```
# 作用域链

  > 资料来自[JavaScript深入之作用域链](https://github.com/mqyqingfeng/Blog/issues/6)
  
  当查找变量的时候，会先从当前上下文的变量对象中查找，如果没有找到，就会从父级(词法层面上的父级)执行上下文的变量对象中查找，一直找到全局上下文的变量对象，也就是全局对象。这样由多个执行上下文的变量对象构成的链表就叫做作用域链。

 **函数创建**
 
  函数内部有一个属性[[scope]], 当函数创建的时候，就会将所有父变量对象保存在里面。
  
  **函数调用**
  
  当函数被调用时，进入函数执行上下文，创建 VO/AO 后，就会将活动对象添加到作用链的前端。
  
  执行上下文的作用域链Scope
  
  ```javascript
    Scope = [AO].concat([[Scope]]);
  ```
  执行过程：
  
  1.函数创建，保存作用域链在内部属性[[scope]]中。
  
  ```javascript
    f.[[scope]] = [
      globalContext.VO
    ]
  ```
  2.函数执行，创建函数执行上下文，并压到执行上下文栈中
  
  ```javascript
    ECStack = [
        fContext,
        globalContext
    ];
  ```
  3.函数执行前准备工作
  
  - 复制函数[[scope]]属性创建作用域链
  
  ```javascript
    fContext = {
      Scope = f.[[scope]]
    }
  ```
  - 用arguments创建活动对象，随后初始化活动对象(AO)，加入形参、函数声明、变量声明
  
  ```javascript
    fContext = {
      AO: {
        arguments: {
          length: 0
        },
        variable: undefined
      },
      Scope: f.[[scope]] // 作用域链
    }
  ```
  
  - 将活动对象压入函数作用域链顶端
  
  ```javascript
    fContext = {
      AO: {
        arguments: {
          length: 0
        },
        variable: undefined
      },
      Scope: [AO, [[scope]]]
    }
  ```
  
  4.执行函数，修改AO的值
 
  ```javascript
    fContext = {
      AO: {
        arguments: {
          length: 0
        },
        variable: <value>
      },
      Scope: [AO, [[scope]]]
    }
  ```
  5.函数执行结束，函数执行上下文从执行栈中弹出
  
  ```javascript
    ECStack = [
      globalContext
    ]  
  ```
