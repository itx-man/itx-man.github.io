---
layout:     post                            # 使用的布局 (不需要修改)
title:      效率工具飞行手册                 # 标题
subtitle:   Atom-Sublime Text-常用的快捷键   # 副标题
date:       2018-10-15                      # 时间
author:     YuanLe                          # 作者
header-img: img/Romantic sunrise.jpg          # 这篇文章标题背景图片
catalog: true                               # 是否归档
tags:                                       # 标签
    - Atom
---


## Atom

来源：[Atom Flight Manual](https://github.com/atom-china/manual)

### 为什么选择Atom？

>关键词：易用性的同时提供充分的可拓展性（hackability）

这个世界上有那么多种编辑器，为什么你要花时间学习和使用 Atom 呢？

虽然 Sublime 和 TextMate 之类的编辑器已经非常好用了，但它们仅提供了很有限的拓展性。而在另一个极端，Emacs 和 Vim 提供了灵活的拓展性，但它们并不是很友好，需要使用专用的编程语言来配置和拓展。

我们觉得我们可以做得更好。我们的目标是在保证易用性的同时提供充分的可拓展性（hackability）：这个编辑器会受到第一天学习编程的新生欢迎，而且当他们成长为编程专家时也难以割舍。

当我们使用 Atom 来开发 Atom 的时候，随着它的逐渐完善，我们愈发觉得已经离不开它了。从表面上来看，Atom 是一个能满足你的期待的，现代化的桌面文本编辑器，而在表面之下，这是一个值得你去一同完善的系统。

### Atom 的核心

Web 技术虽然有其缺陷，但经过二十年的发展，Web 已经逐渐成长为了一个强大的具有活力的平台。所以当我们计划写一个自用的可拓展的文本编辑器时，Web 技术显然是一个好的选择，但首先我们需要摆脱来自 Web 的限制。

### 混合本地代码与 Web 技术

Web 浏览器很适合用来浏览网页，但写代码是一种需要可靠的工具的专业活动。更重要的是，浏览器出于安全的考虑，严格限制了对本地系统的访问，但对一个文本编辑器而言，不能向本地系统写入文件是不可接受的。

因此，我们没有把 Atom 构建为一个传统的 Web 应用，Atom 是一个专门被设计用作文本编辑器，而不是网页浏览器的 Chromium 定制版。Atom 的每一个窗口实际上都是一个本地渲染的网页。

所有来自 Node.js 可用的 API 在 Atom 窗口的 JavaScript 中同样可用，这种结合带来了一种独一无二的开发体验。

因为一切都是本地的，你不需要将静态资源打包、不需要关注脚本的异步加载，如果你希望加载一些代码。只需要在文件的最顶部 require 它即可，Node.js 的模块系统允许你将一个系统分割为小的、专注于某一功能的包。

### JavaScript 与 C++ 的结合

与原生代码交互也很简单。例如，你基于 Oniguruma 正则引擎开发了一个用来提供对 TextMate 语法识别的支持。在浏览器里，你可能需要使用 NaCl 或 Esprima, 而在 Node 里这个过程变得非常简单。

在 Node.js 的 API 之外，我们还提供了一些 API 例如使用系统的对话框、使用菜单栏和右键菜单、操纵窗口尺寸等等。

### Web 技术：最有趣的部分

另一个好消息就是当你为 Atom 编写代码时，这些代码一定会被允许在最新版本的 Chromium 中。这意味着你可以无视与浏览器兼容性有关的黑科技，使用全部的最新的 Web 功能。

例如，Atom 的工作区和窗格都是基于 flexbox 来进行布局的。这是一项刚刚出现的技术，从我们使用它之后也发生了很多变化，但不要紧，因为它工作得很好。

我们确信将 Atom 构建在 Web 技术之上是一个好的选择，因为整个行业都在推动着 Web 技术的发展。原生UI技术不断产生又不断淘汰，而 Web 是一个每年都变得更加强大和普及的标准。我们对于深入探索这一强大的技术感到无比兴奋。

### 一个开源的文本编辑器

GitHub 的目标是帮助大家构建更好的软件，而 Atom 则是实现这一目标的重要补充。Atom 是一项长期的投资，GitHub 会持续投入开发力量来推动它的发展。

但我们也意识到不能让它受限于我们的能力，就像之所以 Emacs 和 Vim 在过去的三十年间被广泛使用，是因为只有开源，才能构建一个持久的、有活力的文本编辑器社区。

### 快捷键

#### 文件切换

| 快捷键 | 快捷键的功能 |
| ------ | ------ |
| cmd-shift-o | 打开目录 |
| cmd-\ | 显示或隐藏目录树 |
| ctrl-0 | 	焦点移到目录树 注意这里是数字 0 非常实用也可以用 cmd+\来变相达到效果 |
| 使用a，m，delete | 	目录树下，使用a，m，delete来增加，修改和删除 |
| cmd-t 或 cmd-p | 查找文件 |
| cmd-b | 在打开的文件之间切换 |
| cmd-shift-b | 只搜索从上次 git commit 后修改或者新增的文件 |

#### 导航

| 快捷键 | 快捷键的功能 |
| ------ | ------ |
| ctrl-p | 前一行 |
| ctrl-n | 后一行 |
| ctrl-f | 前一个字符 |
| ctrl-b | 后一个字符 |
| alt-B, alt-left |	移动到单词开始 |
| alt-F, alt-right |	移动到单词末尾 |
| cmd-right, ctrl-E	| 移动到一行结束 |
| cmd-left, ctrl-A	| 移动到一行开始 |
| cmd-up	| 移动到文件开始 |
| cmd-down	| 移动到文件结束 |
| ctrl-g	| 移动到指定行 row:column 处 |
| cmd-r	| 在方法之间跳转 |

#### 选取

| 快捷键 | 快捷键的功能 |
| ------ | ------ |
| ctrl-shift-P |	选取至上一行 |
| ctrl-shift-N	| 选取至下一样 |
| ctrl-shift-B |	选取至前一个字符 |
| ctrl-shift-F	| 选取至后一个字符 |
| alt-shift-B, alt-shift-left |	选取至字符开始 |
| alt-shift-F, alt-shift-right |	选取至字符结束 |
| ctrl-shift-E, cmd-shift-right	| 选取至本行结束 |
| ctrl-shift-A, cmd-shift-left |	选取至本行开始 |
| cmd-shift-up |	选取至文件开始 |
| cmd-shift-down |	选取至文件结尾 |
| cmd-A |	全选 |
| cmd-L	| 选取一行，继续按回选取下一行 |
| ctrl-shift-W | 选取当前单词 |

#### 目录树操作

| 快捷键 | 快捷键的功能 |
| ------ | ------ |
| cmd-\ 或者 cmd-k cmd-b | 显示(隐藏)目录树 |
| ctrl-0 | 焦点切换到目录树(再按一次或者Esc退出目录树) |
| 在目录下 |
| a	| 添加文件 |
| d	| 将当前文件另存为(duplicate) |
| i	| 显示(隐藏)版本控制忽略的文件 |
| alt-right 和 alt-left	| 展开(隐藏)所有目录 |
| ctrl-al-] 和 ctrl-al-[	| 同上 |
| ctrl-[ 和 ctrl-]	| 展开(隐藏)当前目录 |
| cmd-k h 或者 cmd-k left	| 在左半视图中打开文件 |
| cmd-k j 或者 cmd-k down	| 在下半视图中打开文件 |
| cmd-k k 或者 cmd-k up	| 在上半视图中打开文件 |
| cmd-k l 或者 cmd-k right	| 在右半视图中打开文件 |
| ctrl-shift-C	| 复制当前文件绝对路径 |

#### 分屏操作

| 快捷键 | 快捷键的功能 |
| ------ | ------ |
| cmd-k h 或者 cmd-k left	| 在左半视图中打开文件 |
| cmd-k j 或者 cmd-k down	| 在下半视图中打开文件 |
| cmd-k k 或者 cmd-k up	| 在上半视图中打开文件 |
| cmd-k l 或者 cmd-k right |	在右半视图中打开文件 |
| cmd-k cmd-k 或者 cmd-k cmd-right |	在右半视图中打开文件 |

#### 书签

| 快捷键 | 快捷键的功能 |
| ------ | ------ |
| cmd-F2	| 在本行增加书签 |
| F2	| 跳到当前文件的下一条书签 |
| shift-F2	| 跳到当前文件的上一条书签 |
| ctrl-F2	| 列出当前工程所有书签 |

#### 编辑和删除文本

| 快捷键 | 快捷键的功能 |
| ------ | ------ |
| ctrl-T |	使光标前后字符交换 |
| cmd-J	 | 将下一行与当前行合并 |
| ctrl-cmd-up, ctrl-cmd-down |	使当前行向上或者向下移动 |
| cmd-shift-D	 | 复制当前行到下一行 |


#### Atom大小写转换

| 快捷键 | 快捷键的功能 |
| ------ | ------ |
| cmd-K, cmd-U	| 使当前字符大写 |
| cmd-K, cmd-L |	使当前字符小写 |

#### 删除和剪切

| 快捷键 | 快捷键的功能 |
| ------ | ------ |
| ctrl-shift-K | 删除当前行 |
| cmd-backspace	| 删除到当前行开始 |
| cmd-fn-backspace |	删除到当前行结束 |
| ctrl-K | 剪切到当前行结束 |
| alt-backspace 或 alt-H |	删除到当前单词开始 |
| alt-delete 或 alt-D | 删除到当前单词结束 |

#### 多光标和多处选取

| 快捷键 | 快捷键的功能 |
| ------ | ------ |
| cmd-click	| 增加新光标 |
| cmd-shift-L	| 将多行选取改为多行光标 |
| ctrl-shift-up, ctrl-shift-down	| 增加上（下）一行光标 |
| cmd-D	| 选取文档中和当前单词相同的下一处 |
| ctrl-cmd-G	| 选取文档中所有和当前光标单词相同的位置 |

#### 括号跳转

| 快捷键 | 快捷键的功能 |
| ------ | ------ |
| ctrl-m	| 相应括号之间，html tag之间等跳转 |
| ctrl-cmd-m	| 括号(tag)之间文本选取 |
| alt-cmd-.	| 关闭当前XML/HTML tag |

#### 编码方式

| 快捷键 | 快捷键的功能 |
| ------ | ------ |
|  ctrl-shift-U | 调出切换编码选项 |

#### 查找和替换

| 快捷键 | 快捷键的功能 |
| ------ | ------ |
| cmd-F |	在buffer中查找 |
| cmd-shift-f	| 在整个工程中查找 |

#### 代码片段

| 快捷键 | 快捷键的功能 |
| ------ | ------ |
| alt-shift-S | 查看当前可用代码片段 |

>在~/.atom目录下snippets.cson文件中存放了你定制的snippets

#### 折叠

| 快捷键 | 快捷键的功能 |
| alt-cmd-[	| 折叠 |
| alt-cmd-]	| 展开 |
| alt-cmd-shift-{	| 折叠全部 |
| alt-cmd-shift-}	| 展开全部 |
| cmd-k cmd-N	| 指定折叠层级 N为层级数 |

#### 文件语法高亮

| 快捷键 | 快捷键的功能 |
| ctrl-shift-L | 选择文本类型 |

#### 使用Atom进行写作

| 快捷键 | 快捷键的功能 |
| ctrl-shift-M | Markdown预览 |

#### git操作

| 快捷键 | 快捷键的功能 |
| cmd-alt-z checkout	| HEAD 版本cmd-shift-B 弹出untracked 和 modified文件列表 |
| alt-g down alt-g up	| 在修改处跳转 |
| alt-G D	| 弹出diff列表 |
| alt-G O	| 在github上打开文件 |
| alt-G G	| 在github上打开项目地址 |
| alt-G B	| 在github上打开文件blame |
| alt-G H	| 在github上打开文件history |
| alt-G I	| 在github上打开issues |
| alt-G R	| 在github打开分支比较 |
| alt-G C |	拷贝当前文件在gihub上的网址 |


## 环境变量

#### Windows环境变量配置

> 1. pwd -> 查看配置路径 path
> 2. echo %PATH% -> 查看环境变量
> 3. set PATH= %PATH%;path


## Cmder

#### 安装

* 下载链接 [cmder.net](http://cmder.net/)

#### 配置环境变量

* 变量名: CMDER_HOME
* 变量值: 安装的绝对路径
* set PATH= %PATH%;%CMDER_HOME%

#### 添加cmder到右键菜单

* Cmder.exe /REGISTER ALL

#### 外观配置

* Font
* Color Schemes
* Quake Style

#### Cmder常用快捷键

| 快捷键 | 快捷键的功能 |
| Tab | 自动路径补全 |
| Ctrl-T | 新建标签页 |
| Ctrl-W | 关闭标签页 |
| Ctrl-Tab | 切换标签页 |
| Alt-F4 | 关闭所有标签页 |
| Alt-Shift-1 | 开启cmd.exe |
| Alt-Shift-2 | 开启powershell.exe |
| Alt-Shift-3 | 开启powershell.exe(系统管理员权限) |
| Ctrl-1 | 快速切换到第1个标签页 |
| Ctrl-n | 快速切换到第n个标签页 |
| Alt-enter | 切换到全屏状态 |
| Ctrl-r | 历史命令搜索 |
| Ctrl-、| Quake Style调出窗口 |
| Win-Alt-p | 设置 |

#### 解决中文乱码

* Settings->Startup->Environment->添加
* set LANG=zh_CN.UTF-8
* set LC_ALL=zh_CN.utf8

## Stetho - Android 调试

* chrome://inspect/#devices --chrome网页打开
* implementation 'com.facebook.stetho:stetho:1.3.1' --加入build.gradle
* Stetho.initializeWithDefaults(this); --application中进行初始化
* Devices -> inspect
