# 回流Reflow重绘Repaint

> 资料来源[https://developers.google.com/web/fundamentals/performance/critical-rendering-path/render-blocking-css?hl=zh-cn](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/render-blocking-css?hl=zh-cn)


浏览器渲染过程
- 构建对象模型    
  根据HTML和CSS生成``DOM(Document Object Model)``树和``CSSOM(CSS Object Model)``树
- DOM树和CSSOM树合并生成渲染树

```HTML
                                ┌───────┐
                                |  DOM  |
                                └───────┘
                                    ↓
  ┌────────┐    ┌──────────┐    ┌────────┐ 
  |  HTML  │ —> │  HTML    | —> │  DOM   |         ┌──────────┐
  └────────┘    |  Parser  |    |  Tree  |         |  Layout  | 
                └──────────┘    └────────┘         └──────────┘
                                    ↓                   ↓↑  Reflow
                              ┌──────────────┐    ┌────────────┐    ┌───────┐   ┌─────────┐
                              |  Attachment  | —> | RenderTree | —> | Paint | —>| Display |  
                              └──────────────┘    └────────────┘    └───────┘   └─────────┘
                                    ↑
  ┌────────┐    ┌──────────┐    ┌─────────┐
  | Style  │ —> │  CSS     | —> │  CSSOM  |  
  | Sheets |    |  Parser  |    |  Tree   |
  └────────┘    └──────────┘    └─────────┘

```


## 回流Reflow

当浏览器必须重新处理和绘制部分或全部页面时，``回流``就会发生。

**导致回流操作**
- 调整窗口大小
- 改变字体
- 增加或移除样式表(stylesheet)
- 内容变化，（在输入框中输入文字）
- 激活CSS伪类(:hover)
- 操作class属性
- 操作DOM
- 计算``offsetWidth``和``offsetHeight``
- 设置style属性的值
 
**避免产生回流**
- 改变元素的类名（尽量在DOM树的最末端）
- 避免设置多项内联样式
- 动画效果应用到``position``属性为``absolute``或者``fixed``的元素上
- 牺牲平滑度换取速度
- 避免使用``Table``布局
- 避免使用CSS的JavaScript表达式（仅IE浏览器）
- 引入外部链接的样式表文件最好放在头部

## 重绘Reflow
