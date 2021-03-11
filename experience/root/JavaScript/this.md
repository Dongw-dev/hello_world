# this

在传统面向对象语言中，例如：Java，``this``关键字用来示当前对象本身，或者当前对象的一个实例。通过``this``可以获得当前对象属性和调用方法。

在Javascript中，函数的调用方式决定了 this 的值（运行时绑定）。``this``不能在执行期间被赋值，并且在每次函数被调用时 this 的值也可能会不同。

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

**函数上下文**

