---
title: css box布局和flex布局
date: 2020-03-11
tags: CSS
---

CSS3 让网页能够符合多种平台，让网页更加的弹性推出了 Flex 布局。网上其实很多带原理的解释，但是不自己上手还是会觉得理解不够清晰，所以我打算以自己的代码示例记录下学习的布局。
## flex
css3新增了display:flex,它使得网页更加弹性适合各种不同的屏幕，它的思想是让容器有能力让其子项目能够改变其宽度、高度甚至是顺序，以最佳的方式填充可用空间。先来写个盒子试试看
display: flex;
justify-content: space-between;
当给外部盒子添加flex属性和间隔属性后呈现