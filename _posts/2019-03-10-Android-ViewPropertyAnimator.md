---
layout:     post                            # 使用的布局 (不需要修改)
title:      属性动画Property Animation（上手篇）                   # 标题
subtitle:   HenCoder               # 副标题
date:       2019-03-10                      # 时间
author:     YuanLe                          # 作者
header-img: img/United States, North America.jpg         # 这篇文章标题背景图片
catalog: true                               # 是否归档
tags:                                       # 标签
    - Android
---

来源：[HenCoder Android 自定义 View 1-6：属性动画 Property Animation（上手篇）](https://hencoder.com/ui-1-6/)

# ![Table of Contents](https://itx-man.github.io/img/toc.png)

<!-- vim-markdown-toc GFM -->

* [属性动画](#属性动画)
    * [简介](#简介)
    * [ViewPropertyAnimator](#ViewPropertyAnimator)
    * [ObjectAnimator](#ObjectAnimator)

<!-- vim-markdown-toc -->

## 简介
Android 里动画可以分为两类，Animation 和 Transition; 其中 Animation 又可以再分为 View Animation 和 Property Animation 两类：View Animation 是纯粹基于 framework 的绘制转变，比较简单；Property Animation，属性动画，这是在 Android 3.0 开始引入的新的动画形式，不过说它是新的只是相对的，它已经有好几年的历史了，而且现在的项目中的动画 99% 都是用的它，极少再用到 View Animation 了。属性动画不仅可以使用自带的API来实现最常用的动画，而且通过自定义 View 的方式来做出定制化的动画。除了这两种 Animation，还有一类动画是 Transition。Transition 这个词的本意是转换，在 Android 里指的是切换界面时的动画效果，这个在逻辑上要复杂一点，不过它的重点在于切换而不是动画。

## ViewPropertyAnimator

> 使用方式：View.animate() 后跟 translationX() 等方法，动画会自动执行。

```
view.animate().translationX(500);
```
![translationX](https://itx-man.github.io/img/source/ViewPropertyAnimatorTranslationX.gif)

具体可以跟 View 中的方法以及所对应的 View 中的实际操作的方法如下图所示：

![ViewPropertyAnimator](https://itx-man.github.io/img/source/ViewPropertyAnimator.jpg)
