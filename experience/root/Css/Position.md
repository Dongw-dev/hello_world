# Position

> 资料来源[MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/position)

``Position``属性是用于指定元素在文档流中的定位方式。

- 定位类型

  - 定位元素
    
    指计算后位置属性为relative, absolute, fixed, sticky中的其中一个元素。
    
  - 相对定位元素-relative
    
    元素先放置在未添加定位时的位置，在不改变页面布局情况下改变位置，不影响其他元素偏移。
    
    对table-\*-group, table-cell, table-row, table-caption无效。
    
  - 绝对定位元素-absolute/fixed
    
    **absolute**    
    ``position``属性为``absolute``的元素被移出文档流，通过指定元素相对于最近的非static的父级元素的偏移量来定位，如果不存在父级元素则指定为初始包含块。
    
    **fixed**    
    同绝对定位一样，固定定位也是脱离文档流，但是元素包含块是viewport视口。    
    ``fixed``属性会生成新的层叠上下文。    
    当父级元素的``transform``, ``perspective``或``filter``属性为非none的时候，容器由viewport视口改为该父级元素。
    
    
    PS: 
    
      被指定为绝对定位的元素可以通过指定``top``和``bottom``来填充可用垂直距离，也可以通过``left``和``right``来填充水平距离。    
      如果``top``和``bottom``都被指定，``top``优先。
    
  - 粘性定位元素-sticky
    
    元素根据正常文档流定位，然后根据最近的滚动父级元素(overflow:``hidden``|``scroll``|``auto``或``overlay``)和块级元素来进行偏移，不会影响其他元素。
    
    ``sticky``总是会创建一个新的层叠上下文。
