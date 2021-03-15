# this

在传统面向对象语言中，例如：Java，``this``关键字用来示当前对象本身，或者当前对象的一个实例。通过``this``可以获得当前对象属性和调用方法。

在Javascript中，函数的调用方式决定了 this 的值(运行时绑定)。``this``不能在执行期间被赋值，并且在每次函数被调用时``this``的值也可能会不同。

ES5 引入了 bind 方法来设置函数的 this 值，而不用考虑函数如何被调用的。

ES2015 引入了箭头函数，箭头函数不提供自身的``this`` 绑定（``this``的值将保持为闭合词法上下文的值）。

**全局上下文**

无论是否在严格模式下，在全局执行上下文中，``this``都指向全局全局对象

```javascript
  // 在浏览器中，this等于window对象
  console.log(this === window) // true
  
  // 如果你声明一些全局变量，这些变量都会作为this的属性。
  a = 37;
  console.log(window.a); // 37

  this.b = "MDN";
  console.log(window.b)  // "MDN"
  console.log(b)         // "MDN"
```

> Note: 你可以使用[globalThis](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/globalThis)获取全局对象，无论你的代码是否在当前上下文运行。

```javascript
  console.log(globalThis === this) // true
  
  // globalThis 提供了一个标准的方式来获取不同环境下的全局 this  对象
  var getGlobal = function () {
    if (typeof self !== 'undefined') { return self; }
    if (typeof window !== 'undefined') { return window; }
    if (typeof global !== 'undefined') { return global; }
    throw new Error('unable to locate global object');
  };
  
```

**函数上下文**

在函数内部，``this``值取决于函数被调用的方式

- 直接调用
  ``this``直接指向全局变量
  
  **非严格模式**    
  ```javascript
    function f1(){
      return this;
    }
    //在浏览器中：
    f1() === window;   //在浏览器中，全局对象是window

    //在Node中：
    f1() === globalThis;
  ```
  
  **严格模式**    
  在严格模式下，如果进入执行环境时没有设置``this``的值，``this``会保持为``undefined``
  ```javascript
    function f2(){
      "use strict"; // 这里是严格模式
      return this;
    }

    f2() === undefined; // true
  ```
  
- call()、apply()

  ``this``指向绑定的对象上。
  
  ```javascript
    var obj = {
      name: 'test'
    };
    function say(age){
      console.log(this.name+'-'+age)
    }
    /** 
      第一个参数用作this对象
      其余参数用作函数参数
     */
    say.call(obj, '23')  // 'test-23'
    /** 
      第一个参数用作this对象
      第二个参数是一个数组，数组中的值用作函数参数
     */
    say.apply(obj, ['23']) // 'test-23'
  ```
  
- bind()

  **ECMAScript 5** 引入了``Function.prototype.bind()``。
  
  调用``f.bind(someObject)``会创建一个与``f``具有相同函数体和作用域的**函数**， ``this``永久绑定在``bind``的第一个参数。
  
  ```javascript
    var obj = {
      name: 'test'
    };
    function say(age){
      console.log(this.name+'-'+age)
    };
    
    var f = say.bind(); // return new function()
    console.log(f()); // 'test-23'
  ```
- 箭头函数

- 作为对象的方法

  当函数作为对象里的方法被**调用**，``this``被设置为指向该函数的对象。
  
  ```javascript
    var obj = {
      name: 'test',
      say: function() {
       console.log(this.name)
      }
    };
    obj.say() // 'test'
  ```
  
  
  同样适用此种规则的两种情况：
  - 原型链中的``this``
  
    ```javascript
      var obj =  {
        f: function(){
          console.log(this.name);
        }
      };
      var testObj = Object.create(obj);
      testObj.name = 'test';
      console.log(testObj.f()); // 'test'
    ```
  - getter 与 setter 中的``this``
    
    ```javascript
      var test = function(){
        console.log(this.name)
      };
      var obj = {
        name: 'test',
        // get语法将对象属性绑定到查询该属性时将被调用的函数。
        get call() {
          console.log(this.name);
        }
      }
      // Object.defineProperty()方法会直接在一个对象上定义一个新属性，或者修改一个对象的现有属性，并返回此对象。
      // 默认情况下，使用 Object.defineProperty() 添加的属性值是不可修改（immutable）的。
      Object.defineProperty(obj, 'shout', {
        get: test, enumerable: true, configurable: true
      });
      console.log(obj.shout) // 'test'
    ```
    
- 构造函数
  
  当一个函数用作构造函数时(使用new关键字)，``this``被被绑定到正在构造的新对象。
  
  构造函数创建对象步骤：
  - 创建新对象
  - this指向新对象
  - 给对象赋值
  - 返回this
  
  ```javascript
    function obj() {
      this.name = 'test'
    }
    var o = new obj();
    console.log(o.name); // 'test'
    
    // 构造函数手动返回对象
    funtion obj() {
      this.name = 'test';
      return {name: 'test001'};
    }
    var o = new obj();
    console.log(o.name); // test001
    
  ```
  
  
  
- DOM事件处理函数
- 内联事件处理函数
- 类中的this

  
