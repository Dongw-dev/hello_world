# Experience

## 原型链

每个对象都会在内部初始化一个属性(prototype),当我们访问对象属性时,如果这个对象内部不存在这个属性,就会去prototype里面找这个属性,这个prototype又有自己的prototype,直至检索到Object内建对象

```javascript
instance.constructor.prototype = instance._proto_
```

## 作用域链

## 闭包

如何理解闭包

1.当一个函数返回值是另一个函数,返回的那个函数调用了其父类函数内部的其他变量,如果返回的这个函数在外部被执行，就形成闭包

2.函数外部能调用函数内部定义变量

3.函数内部可以读取函数外部的全局变量,在外部无法读取函数内的局部变量

4.在退出函数之前,将不使用的局部变量全部删除

## 跨域请求

跨域请求方法

1.proxy代理

通过nginx代理将请求发送给后台服务器，通过服务器来发送请求,然后将请求结果传给前端

2.CORS[Cross-Origin Resource Sharing]

后端在处理请求数据的时候，添加允许跨域的相关操作。

```javascript
{
    "Content-Type": "text/html; charset=UTF-8",
    "Access-Control-Allow-Origin":'http://localhost',
    'Access-Control-Allow-Methods': 'GET, POST, OPTIONS',
    'Access-Control-Allow-Headers': 'X-Requested-With, Content-Type'
}
```

3.jsonp

通过动态插入script标签, 浏览器对script资源引用没有同源限制, 资源加载到页面后会立即执行
---无法发送post请求

```javascript
var _script = document.createElement('script');
_script.type = "text/javascript";
_script.src = "http://localhost:8888/jsonp?callback=testjsonp";
document.head.appendChild(_script);
```

## 开发及性能优化

## 内存泄漏

内存泄漏定义

>一块被分配的内存既不能使用,又不能回收,直至浏览器进程结束。

内存泄漏情况

1.页面元素被移除或者替换时，元素绑定事件没被移除

eg:

```javascript
var btn = document.getElementById("myBtn");
btn.onclick = function(){
document.getElementById("myDiv").innerHTML =      "Processing...";
//处理方式：btn.onclick = null;
    }
```

2.函数内定义函数，并且内部函数事件回调引用外暴，形成闭包,闭包可以维持函数内局部变量,使其得不到释放

```javascript
function bindEvent(){
    var obj=document.createElement("XXX");
    obj.onclick=function(){
        //Even if it's a empty function
    }
    // 解决方法:obj = null;
}
```

## 正则表达式

## 浏览器兼容

## 浏览器工作原理

1.地址加载到完成发生什么事情：

```
-> 发起请求：URL解析/DNS解析
-> 网络连接：3次握手
-> 服务器响应请求:返回数据 
-> 客户端接收响应：浏览器加载/渲染页面
-> 呈现页面
```


## 浏览器内核的理解

## cookies，sessionStorage 和 localStorage 的区别

## 罗马数字转换
```javascript
function convert(num) {
  var lookup ={M:1000,CM:900,D:500,CD:400,C:100,XC:90,L:50,XL:40,X:10,IX:9,V:5,IV:4,I:1};
  var romanStr = "";
  for (var i in lookup){
    while (num >= lookup[i]){
      romanStr+=i;
      num -= lookup[i];
    }
  }
  return romanStr;
}
## 手机分辨率dpr计算

## 首屏优化加载

- 减少首屏CGI的计算量
- 页面瘦身：压缩HTML,CSS,Javasript
- 减少请求：CSS，Javascript文件数尽量少
- 多用缓存
- 少用table布局
- 做预加载：用loading动画做过渡
