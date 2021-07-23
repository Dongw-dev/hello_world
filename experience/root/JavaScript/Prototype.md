# Prototype - 原型

> 资料来源：[https://mp.weixin.qq.com/s/1UDILezroK5wrcK-Z5bHOg](https://mp.weixin.qq.com/s/1UDILezroK5wrcK-Z5bHOg)

## prototype在ES规范中的理解

ES2019的规范中的定义
> object that provides shared properties for other objects.
> 翻译：为其他对象提供共享属性的对象

Prototype就是一个对象，承担了共享属性的作用；当某个对象也产生了同样的作用，它就成了该对象的prototype。

所有对象都可以作为另一个对象的prototype。


- 每个对象都有个prototype隐式引用。

```javascript

  var obj = {
    name: 'test',
    // __proto__不是由开发者创建
    // obj是被隐式的挂载在其他对象的引用，至于__proto__属性中
    __proto__: {} 
  }

```
``prototype``在规范中应是``隐式引用`` 
  - 通过``Object.getPrototypeOf(obj)``获取指定对象的prototype对象。
  - 通过``Object.setPrototypeOf(obj, anotherObj)``设置指定对象的prototype对象。


- __proto__属性
  - 它是一个访问器属性，存在于``Object.prototype``中
  - 访问``obj.__proto__``是通过``Object.prototype``的get/set方法。
  ```javascript
    Object.defineProperty(Object.prototype, '__proto__',{
        get(){
          console.log('get');
        }
    });
  ```
  - 普通对象创建时，只需要将内部隐式引用指向``Object.prototype``，就可以兼容``__proto__``属性访问行为，不需要将原型隐式挂载到对象``__proto__``属性上。

- prototype chain 原型链
  
  规范中定义：
  > a prototype may have a non-null implicit reference to its prototype, and so on; this is called the prototype chain.

  prototype对象如果恰好是另一对象隐式引用的普通对象，那么它也是对象，它也有自己的隐式引用，如此构成对象的原型的原型的链，直到某个对象的隐式引用为null,链条终止。

  **作用**
  
  ```
    访问对象属性时，是通过原型链来查找的。这个对象是第一个元素，首先检查是否有属性名，如果存在则返回，否则检查第二个元素。
  ```

- 原型继承方式
  - 显式原型继承
  - 隐式原型继承



