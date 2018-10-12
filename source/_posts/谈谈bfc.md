---
title: 谈谈bfc
date: 2017-05-29 12:21:20
tags: 前端
---
BFC(Block Formating Context,块级格式化上下文)
块级格式化上下文就是页面上的一个隔离的渲染区域 容器内部的  
子元素不过在布局上影响容器外的元素  反之也是如此  
如何产生BFC呢？ float的值不为none  overflow值不为visible  position 的值 为 position和fixed  display的值为table-cell  
table-caption inline-block的任何一个   
常见的多栏布局  结合块级元素浮动  里面的元素在一个相对隔离的环境中运行   
说到BFC就要谈谈别的FC的  FC全称Formating Contexts  是W3C 中  CSS2.1规范中一个概念  他是页面中一块渲染区域有一套渲染规则 决定子元素如何定位 以及其他元素的关系和相互作用   
IFC(Inline Formating Contexts)直译为"内联格式化上下文"  
设置为display:inline-block会在外层产生IFC  可通过text-align:center使其水平居中   
GFC(GridLayout Formating Contexts,网格布局上下文)  
当一个元素display设置为grid时  此元素获得一个独立渲染的区域     
我们可以通过在网格容器（grid container）上定义网格定义行（grid      definition rows）和网格定义列（grid definition columns）属性  各在网格项目（grid item）上定义网格行（grid row）和网格列（grid      columns）为每一个网格项目（grid item）定义位置和空间。
FFC(Flex Formatting Contexts,自适应格式化上下文)  display:flex和inline-flex 会生成自适应容器   
 