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
    /**
     *  模拟call函数
     *  
     */
    Function.prototype.callProto = function(context){
      // 当传入null或undefined，指向window
      var context = context || window;
      
      // 获取调用call的函数本身
      context.fn = this;
      
      // 传入的参数并不确定, 需要从arguments对象中取出参数
      // arguments是一个传递给函数的参数的类数组对象，本身不是个Array对象，但是有length和索引长度属性，可被转换为Array对象。
      var args = [];
      for(var i = 1; i < arguments.length;i++ ) {
        args.push(`arguments[${i}]`);
      }
      
      // context.fn();
      // eval()会将传入的字符串当做JavaScript代码来执行
      // eval('context.fn(' + args + ')');
      // 当调用call的函数有返回值时
      var result = eval('context.fn(' + args + ')');
      
      delete context.fn
      return result; 
    }
  ```
  
## apply

与call类似，区别是：
  - ``call``的参数是一个列表，将每个参数一个个列出来
  - ``apply``的参数是一个数组，将每个参数放到一个数组中
  
**语法：**
  
```javascript
  /**
   * argsArray： 参数数组
   */
  function.apply(thisArg, [argsArray])
```

**模拟实现**

```javascript
  /**
   *  模拟apply函数
   *  
   */
  Function.prototype.applyProto = function(context){
    var context = context || window;
    context.fn = this;
    var args = arguments[1];
    var result;
    if(result == undefined) {
      result = context.fn()
    } else {
      var fnArr = [];
      for(var i = 0; i < args.length;i++){
        fnArr.push('args[' + i + ']');
      }
      result = eval('context.fn(' + fnArr + ')');
    }
    delete context.fn
    return result;
  }
```

## bind

``bind()``方法会创建一个新函数。当这个新函数被调用时，``bind()``的第一个参数将作为它运行时的``this``，之后的一序列参数将会在传递的实参前传入作为它的参数

**语法**

```javascript
function.bind(thisArg[, arg1[, arg2[, ...]]])
```

