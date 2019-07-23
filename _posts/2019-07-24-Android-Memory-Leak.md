---
layout:     post                            # 使用的布局 (不需要修改)
title:      Android内存泄漏分析               # 标题
subtitle:   内存泄漏                   # 副标题
date:       2019-07-24                      # 时间
author:     YuanLe                          # 作者
header-img: img/Lake Tekapo, New Zealand.jpg         # 这篇文章标题背景图片
catalog: true                               # 是否归档
tags:                                       # 标签
    - 内存泄漏
---

来源：[Android 内存泄漏分析心得](https://zhuanlan.zhihu.com/p/25213586)

# ![Table of Contents](https://itx-man.github.io/img/toc.png)

<!-- vim-markdown-toc GFM -->

* [内存泄漏](#内存泄漏)
    * [前言](#前言)
    * [Java中的内存分配](#Java中的内存分配)
    * [四种引用](#四种引用)

<!-- vim-markdown-toc -->

## 前言

今天，我们来讲讲 Android 内存泄漏。

![question](https://itx-man.github.io/img/source/question.webp)

（内存泄漏是什么呀？）

对于 C++ 来说，内存泄漏就是 new 出来的对象没有 delete;

对于 Java 来说，就是 new 出来的 Object 放在 Heap 上无法被 GC 回收；

---

![leak](https://itx-man.github.io/img/source/Java-Leak.jpg)

**内存泄露**(Memory Leak)是指无用对象（不再使用的对象）持续占有内存，或者无用对象的内存得不到及时释放，从而造成内存空间得不到有效的利用。在Java中，内存泄露的原因，通常是长生命周期的对象，持有短生命周期对象的引用。在《深入理解Java虚拟机》中的解释是，当无用对象一直被有用对象引用，导致无用对象可达，JVM无法对无用对象完成回收。从而造成内存泄露。