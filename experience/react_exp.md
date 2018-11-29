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

3. JSX 允许直接在模板插入 JavaScript 变量。如果这个变量是一个数组，则会展开这个数组的所有成员

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

## 4.组件

React 实用React.createClass方法将代码封装成组件（component），然后在网页中插入这个组件。

```javascript
var HelloMessage = React.createClass({
  render: function() {
    return <h1 className={this.props.className}>Hello {this.props.name}</h1>;
  }
});

ReactDOM.render(
  <HelloMessage name="John" className="content__color"/>,
  document.getElementById('example')
);
```
PS：
1. 组件类的第一个字母必须大写
2. 组件类只能包含一个顶层标签
3. 所有组件类都必须有自己的 render 方法，用于输出组件
4. 添加组件属性, class 属性需要写成 className ，for 属性需要写成 htmlFor ，这是因为 class 和 for 是 JavaScript 的保留字
5. this.props.children
- this.props.children的值有三种情况:如果当前组件没有子节点，它就是 undefined ;如果有一个子节点，数据类型是 object ；如果有多个子节点，数据类型就是 array
- 处理方法：React提供方法 React.Children 来处理 this.props.children 。我们可以用 React.Children.map 来遍历子节点

```javascript
var NotesList = React.createClass({
  render: function() {
    return (
      <ol>
      {
        React.Children.map(this.props.children, function (child) {
          return <li>{child}</li>;
        })
      }
      </ol>
    );
  }
});
ReactDOM.render(
  <NotesList>
    <span>hello</span>
    <span>world</span>
  </NotesList>,
  document.body
);
```

6. PropTypes:
- 用来验证组件实例的属性是否符合要求

```javascript
var MyTitle = React.createClass({
  propTypes: {
    title: React.PropTypes.string.isRequired
  },
  render: function() {
     return <h1> {this.props.title} </h1>;
   }
});
```
