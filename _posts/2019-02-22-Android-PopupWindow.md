---
layout:     post                            # 使用的布局 (不需要修改)
title:      PopupWindow(悬浮框)的基本使用                    # 标题
subtitle:   Android基础入门教程                 # 副标题
date:       2019-02-21                      # 时间
author:     YuanLe                          # 作者
header-img: img/Australia, Australia_Pacific.jpg         # 这篇文章标题背景图片
catalog: true                               # 是否归档
tags:                                       # 标签
    - 自定义View
---

来源：[PopupWindow(悬浮框)的基本使用](http://www.runoob.com/w3cnote/android-tutorial-popupwindow.html)

PopupWindow的「定义」：

> 用于显示信息的UI控件——PopupWindow(悬浮框)，如果你想知道 他长什么样子，你可以打开你手机的QQ，长按列表中的某项，这个时候后弹出一个黑色的小 对话框，这种就是PopupWindow了，和AlertDialog对话框不同的是，他的位置可以是随意的；
> 另外AlertDialog是非堵塞线程的，而PopupWindow则是堵塞线程的！而官方有这样一句话来介绍 PopupWindow：
> A popup window that can be used to display an arbitrary view. The popup window is
> a floating container that appears on top of the current activity.
> 大概意思是：一个弹出窗口控件，可以用来显示任意View，而且会浮动在当前activity的顶部


# ![Table of Contents](https://itx-man.github.io/img/toc.png)

<!-- vim-markdown-toc GFM -->

* [相关方法解读](#相关方法解读)
    * [几个常用的构造方法](#几个常用的构造方法)
    * [常用的一些方法](#常用的一些方法)
    * [核心代码](#核心代码)

<!-- vim-markdown-toc -->

## 1.相关方法解读

### 1) 几个常用的构造方法

{% highlight java %}
//我们在文档中可以看到，提供给我们的PopupWindow的构造方法有九种之多，这里只贴实际 开发中用得较多的几个构造方法：

* public PopupWindow (Context context)
* public PopupWindow(View contentView, int width, int height)
* public PopupWindow(View contentView)
* public PopupWindow(View contentView, int width, int height, boolean focusable)

//参数就不用多解释了吧，contentView是PopupWindow显示的View，focusable是否显示焦点
{% endhighlight %}

### 2) 常用的一些方法

{% highlight java %}
//下面介绍几个用得较多的一些方法，其他的可自行查阅文档：

* setContentView(View contentView)：//设置PopupWindow显示的View
* getContentView()：//获得PopupWindow显示的View
* showAsDropDown(View anchor)：//相对某个控件的位置（正左下方），无偏移
* showAsDropDown(View anchor, int xoff, int yoff)：//相对某个控件的位置，有偏移
* showAtLocation(View parent, int gravity, int x, int y)： //相对于父控件的位置（例如正中央Gravity.CENTER，下方Gravity.BOTTOM等），可以设置偏移 或无偏移 PS:parent这个参数只要是activity中的view就可以了！
* setWidth/setHeight：//设置宽高，也可以在构造方法那里指定好宽高， 除了可以写具体的值，还可以用WRAP_CONTENT或MATCH_PARENT， popupWindow的width和height属性直接和第一层View相对应。
* setFocusable(true)：//设置焦点，PopupWindow弹出后，所有的触屏和物理按键都由PopupWindows 处理。其他任何事件的响应都必须发生在PopupWindow消失之后，（home 等系统层面的事件除外）。
                     //比如这样一个PopupWindow出现的时候，按back键首先是让PopupWindow消失，第二次按才是退出 activity，准确的说是想退出activity你得 首先让PopupWindow消失，
                     //因为不并是任何情况下按back PopupWindow都会消失，必须在PopupWindow设置了背景的情况下 。
* setAnimationStyle(int)：//设置动画效果
{% endhighlight %}

### 3) 核心代码

{% highlight java %}
private Button btn_show;
private Context mContext;

@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);
    mContext = MainActivity.this;
    btn_show = (Button) findViewById(R.id.btn_show);
    btn_show.setOnClickListener(new View.OnClickListener() {
        @Override
        public void onClick(View v) {
            initPopWindow(v);
        }
    });
}


private void initPopWindow(View v) {
    View view = LayoutInflater.from(mContext).inflate(R.layout.item_popup, null, false);
    Button btn_xixi = (Button) view.findViewById(R.id.btn_xixi);
    Button btn_hehe = (Button) view.findViewById(R.id.btn_hehe);
    //1.构造一个PopupWindow，参数依次是加载的View，宽高
    final PopupWindow popWindow = new PopupWindow(view,
            ViewGroup.LayoutParams.WRAP_CONTENT, ViewGroup.LayoutParams.WRAP_CONTENT, true);

    popWindow.setAnimationStyle(R.anim.anim_pop);  //设置加载动画

    //这些为了点击非PopupWindow区域，PopupWindow会消失的，如果没有下面的
    //代码的话，你会发现，当你把PopupWindow显示出来了，无论你按多少次后退键
    //PopupWindow并不会关闭，而且退不出程序，加上下述代码可以解决这个问题
    popWindow.setTouchable(true);
    popWindow.setTouchInterceptor(new View.OnTouchListener() {
        @Override
        public boolean onTouch(View v, MotionEvent event) {
            return false;
            // 这里如果返回true的话，touch事件将被拦截
            // 拦截后 PopupWindow的onTouchEvent不被调用，这样点击外部区域无法dismiss
        }
    });
    popWindow.setBackgroundDrawable(new ColorDrawable(0x00000000));    //要为popWindow设置一个背景才有效


    //设置popupWindow显示的位置，参数依次是参照View，x轴的偏移量，y轴的偏移量
    popWindow.showAsDropDown(v, 50, 0);

    //设置popupWindow里的按钮的事件
    btn_xixi.setOnClickListener(new View.OnClickListener() {
        @Override
        public void onClick(View v) {
            Toast.makeText(MainActivity.this, "你点击了嘻嘻~", Toast.LENGTH_SHORT).show();
        }
    });
    btn_hehe.setOnClickListener(new View.OnClickListener() {
        @Override
        public void onClick(View v) {
            Toast.makeText(MainActivity.this, "你点击了呵呵~", Toast.LENGTH_SHORT).show();
            popWindow.dismiss();
        }
    });
}
{% endhighlight %}
