---
layout:     post                            # 使用的布局 (不需要修改)
title:      绘图基础                    # 标题
subtitle:   《Android自定义控件开发入门与实战》_启舰_读书笔记               # 副标题
date:       2019-02-22                      # 时间
author:     YuanLe                          # 作者
header-img: img/United States, North America.jpg         # 这篇文章标题背景图片
catalog: true                               # 是否归档
tags:                                       # 标签
    - 自定义View
---

# ![Table of Contents](https://itx-man.github.io/img/toc.png)

<!-- vim-markdown-toc GFM -->

* [基本图片绘制](#基本图片绘制)
    * [概述](#概述)
    * [画笔的基本设置](#画笔的基本设置)
    * [编辑工具](#编辑工具)

<!-- vim-markdown-toc -->

## 基本图片绘制

### 概述

  生活中美术作图需要两个工具：纸和笔。在Android中，Paint类就是画笔，而Canvas类就是纸，在这里叫做画布。

  所以，凡是跟画笔设置相关的，比如画笔大小、粗细、画笔颜色、透明度、字体的样式等，都在Paint类里设置；同样，凡是要画出成品的东西，比如圆形、
矩形、文字等，都调用Canvas类里的函数生成。
