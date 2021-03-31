# this

> 资料来源    
> [MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/this)    
> [axuebin](https://github.com/axuebin/articles/issues/6)

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
     * 第一个参数用作this对象
     * 其余参数用作函数参数
     */
    say.call(obj, '23')  // 'test-23'
    /** 
     * 第一个参数用作this对象
     * 第二个参数是一个数组，数组中的值用作函数参数
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
  
  在箭头函数中，``this``与封闭词法环境的this保持一致。被设置为全局对象
  
  ```javascript
    var globalObject = this;
    var _this = () => this;
    console.log(globalObject === _this); // true
    
    // 作为对象的一个方法调用
    var obj = {
      test: () => {
        console.log(this);
      }
    };
    console.log(obj.test() === globalObject); // true
    
    // 在其他函数内创建的箭头函数
    // 返回值是箭头函数, this绑定在外层函数中
    var obj = {
      test: function(){
        var _this = (() => this);
        return _this;
      }
    };
    
    // 作为obj对象的一个方法来调用bar，把它的this绑定到obj。
    var fn = obj.test();
    console.log(fn() === globalObject); // false
    console.log(fn() === obj); // true
    
    // 引用obj的方法，没有调用test方法
    // 调用箭头函数后, this指向window，从调用对象获取的当前this指向。
    var fn = obj.test;
    console.log(fn()() === globalObject); // true
  ```
  
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

  当函数用作事件处理函数，this指向触发事件的dom。
  
  > 一些浏览器在使用非 addEventListener 的函数动态地添加监听函数时不遵守这个约定
  
  ```javascript
    var element = document.getElementById('id');
    element.addEventListener('click', function(e){
      console.log(this === e.target); // true
    });
  ```
  
- 内联事件处理函数
  
  被内联``on-event``处理函数 调用时，它的this指向监听器所在的DOM元素。
  
  ```html
    <button onclick="alert(this)">Click Me</button>
    
    <!--没有设置内部函数的this, 将指向默认全局对象global/window-->
    <button onclick="alert((function(){ return this })())">Click Me</button>
  ```
  
- 类中的this

  在类的构造函数中，``this``是一个常规对象。类中所有非静态的方法都会被添加到 this 的原型中。
  
  类中的this取决于如何被调用。
  
  ```javascript
    class Police {
      constructor(){
        // 在构造器中绑定类方法，可以改变this绑定行为。
        this.greet = this.greet.bind(this)
      }
      yell(){
        console.log(this.word)
      }
      greet(){
        console.log(this.word)
      }
      get word(){
        return 'Hello?!' 
      }
    }
    
    class Thief {
      get word(){
        return 'SHIT!!' 
      }
    }
    
    var police = new Police();
    var thief = new Thief();
    
    // 默认情况，this绑定值取决于调用对象
    police.yell() // Hello?!
    thief.yell = police.yell
    thief.yell() // SHIT!!
    
    // bind函数改变this指向对象，让this始终指向最初绑定的对象。
    police.greet() // Hello?!
    thief.greet = police.greet
    thief.greet() // Hello?!
  ```
  
  
