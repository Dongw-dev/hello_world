# call/apply/bind 

## call

使用一个指定的``this``值和单独给出的一个或多个参数来调用一个函数。

``call``是属于所有``Function``的方法

``call()``

**语法**

```javascript

  /**
   * thisArg: 在函数运行时this的值,如果处于非严格模式，指定为null或undefined时候被会自动替换为指向全局对象，原始值会被包装
   * arg1/arg2:  指定参数
   */
  function.call(thisArg, arg1, arg2, ...)
 
```

**模拟实现**
- 实现原理

```javascript
  var obj = {};
  // 将函数设为对象的属性    
  obj.fn = fn
  // 执行该函数    
  obj.fn()
  // 删除该函数    
  delete obj.fn
```

- 具体实现
  
  ```javascript
    Function.prototype.protoCall = function(context){
      // 获取调用call的函数本身
      context.fn = this;
      context.fn();
      delete context.fn
    }
  ```
