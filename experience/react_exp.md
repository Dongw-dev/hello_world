# React

## 1.HTML模板语法

``` javascript

<!DOCTYPE html>
<html>
  <head>
    <script src="../build/react.js"></script>
    <script src="../build/react-dom.js"></script>
    <script src="../build/browser.min.js"></script>
  </head>
  <body>
    <div id="example"></div>
    <script type="text/babel">
      // ** code goes here! **
    </script>
  </body>
</html>

```
PS:

1. < script >标签的type属性为"text/babel"。这是因为React独有的JSX语法，跟 JavaScript不兼容。凡是使用JSX的地方，都要加上type="text/babel"
2. 上面代码一共用了三个库：`react.js` 、`react-dom.js` 和 `Browser.js` ，它们必须首先加载。其中，`react.js` 是 React 的核心库，`react-dom.js` 是提供与 DOM 相关的功能，`Browser.js` 的作用是将 JSX 语法转为 JavaScript 语法，这一步很消耗时间，实际上线的时候，应该将它放到服务器完成。

``` javascript
$ babel src --out-dir build
```

## 2.DOM渲染

ReactDOM.render()用于将模板转为HTML语言,插入指定节点

``` javascript
ReactDOM.render(
  <h1>Hello, world!</h1>,
  document.getElementById('example')
);
```
PS: 

插入的HTML模板不是String类型

## 3.JSX语法

1. HTML 语言直接写在JavaScript语言之中，不加任何引号，它允许 HTML 与 JavaScript 的混写
2. 遇到 HTML 标签（以 < 开头），就用 HTML 规则解析；遇到代码块（以 { 开头），就用 JavaScript 规则解析。

```javascript
var names = ['Alice', 'Emily', 'Kate'];

ReactDOM.render(
  <div>
  {
    names.map(function (name) {
      return <div>Hello, {name}!</div>
    })
  }
  </div>,
  document.getElementById('example')
);
```

3. JSX 允许直接在模板插入 JavaScript 变量。如果这个变量是一个数组，则会展开这个数组的所有成员.

```javascript

var arr = [
  <h1>Hello world!</h1>,
  <h2>React is awesome</h2>
]

ReactDOM.render(
  <div>{arr}</div>,
  document.getElementById('example')
);

```


## 4.组件开发

React 实用React.createClass方法将代码封装成组件（component），然后像插入普通 HTML 标签一样，在网页中插入这个组件。