---
title: 有赞入职半年总结
date: 2016-07-28 22:40:00
tags: Summary
---

今年 1.11 入职有赞，到现在差不多 7 个多月。

很惭愧，只做了一点微小的工作

<!-- more -->

### 统一 Maven 仓库

旧的二方包部署方式非常不专业，统一打包脚本之后，使协作变得高效。

* 所有二方包全部部署到公司内部 Maven 仓库，删掉所有跟随源码的私有仓库，节约 git 服务器空间。
* 把源码、注释、文档全部打进二方包，方便接入方查看源码。
* 坚持代码为中心，文档（README、WIKI）跟着代码走。

> 这是我在有赞做的第一件事

---

### 统一 WebView 框架

重构旧的 WebView 框架，重构之后的优势：

0. 只有 4.2 以下版本使用注入 js 的方式实现 hybrid api，>= 4.2 的版本使用 `@javascriptinterface`，大大降低了旧方式注入失败的概率。
0. 每个 hybrid api 相互独立，独自定义 Model 类。（旧方式是共享一个 Model，导致 Model 无比臃肿）。
0. 注入、回调的接口更加合理。
0. 同时支持新旧协议。
0. 接入 Web Cache。

---

### 引入 RxJava + Retrofit

替换旧的网络框架，在**有赞商家版**中接入 Retrofit & RxJava。

* 输出一篇 [Retrofit + RxAndroid 实践总结](http://www.liangfei.me/2016/07/06/my-practice-with-retrofit-and-rxjava/)。
* 读完了 Retrofit 源码，输出一篇 [图解 Retrofit 之 ServiceMethod](http://www.liangfei.me/2016/07/10/diagram-retrofit-service-method/)。

---

### 排查 Memory Leaks

* 接入 `LeakCanary`，居然有 static 的 view 变量。ありえない！
* 读完了源码 - [LeakCanary 原理分析](https://github.com/LyndonChin/wo/tree/master/leakcanary)。

---

### 开发数据图表库

* 基于 [MPAndroidChart](https://github.com/PhilJay/MPAndroidChart) 二次开发，加了一些特殊交互。
* 花了三周时间彻底搞懂了源码 - [MPAndroidCharts 源码解析](https://github.com/LyndonChin/wo/tree/master/mpandroicharts)。

> MPAndroidCharts 的代码质量跟 star 数完全不匹配

---

### 使用 product flavor 适配 pos 机

* POS 机特有代码互不干扰。
* 减小包大小。

> 只用了一个晚上

---

### 读了很多内部源码

* 商家版 APP 源码
* 基于 okhttp 的网络库
* 23 版本后的权限适配库
* 基于 AndFix 的热修复库
* 一些 UI 库

---

### 做了一些技术分享

0. RxJava 入门
0. GOOGLE/IO 2016
0. 30分钟精通 Retrofit

---

生活变得更加有序，心态要平和。

> 性格を優しくしないと！
