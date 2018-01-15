---
title: 面试问题大汇总（不断更新）
date: 2018-01-15 13:18:49
tags: Android
---

### Android
* Activity & Fragment
    * Activity的生命周期
        * onDestroy一定会被执行吗？
        * onStop中能更新数据吗？
    * Activity的启动模式
        * 什么情况下Activity的onNewIntent会执行？
    * Fragment能否不依赖Activity存在？
    * 描述一下Framgent的栈管理机制？
    * Activity和Fragment如何通信？
    * 如果后台的Activity由于某原因被系统回收了，如何在被系统回收之前保存当前状态？
    * Activity间通过Intent传递数据大小有没有限制？
* Service
    * Service有几种启动方式？
    * Service的生命周期？
    * Service和IntentService的区别是什么？
* ContentProvider & 数据
    * 请介绍下ContentProvider是如何实现数据共享的
    * 请介绍下Android的数据存储方式
* Intent
    * 什么是显示intent（explicit intent）、什么是隐式intent（implicit intent）？
    * intent-filter的组成部分有哪些？
* View & Layout
    * View的绘制流程
    * Touch时间的传递机制
    * Android中的几种动画
        * FrameAnimation
        * TweenAnimation
        * PropertyAnimation
    * Android中常用的布局
        * RelativeLayout & LinearLayout & ConstraintLayout
    * Activity、View、Window之间的关系是什么？
* Android中跨进程通讯有几种方式（**多进程**）
    * 访问其他应用程序的Activity
    * Content Provider
    * 广播（Broadcast）
    * AIDL
* 线程
    * 单线程模型中Message,Handler,Message Queue,Looper之间的关系
    * Thread和HandlerThread的区别是什么？
    * 造成ANR的原因是什么？
* 内存
    * 内存溢出和内存泄漏有什么区别？
    * 何时会产生内存泄漏？
    * 内存优化有哪些方法？
* 设计模式
    * Android 中常见的设计模式有哪些？
    * OkHttp的interceptor是什么设计模式？chain of responsibility
    * 什么是AOP？
* 其他
    * asset 和 raw 文件夹的区别是什么？
    * ApplicationId和PackageName的区别是什么？
    * 说一个jsbridge的实现方式？
    * DVM和JVM的区别是什么？
        * Android 的类加载机制？ClassLoader
    * Android最新版本是什么？有哪些新特性？
    * version_code和version_name的区别是什么？

### 协议
* http的method有哪些？
* https与http的区别是什么？
* 能描述一下TLS握手的过程吗？
* 什么是中间人攻击（MIMA）？
* 什么是对称加密、什么是非对称加密？
* OAuth2的client_id和client_secret是干什么用的？
* 如何在http层面优化网络？

### 工具类
* git怎么暂时保存更改

### Java
* Java中`volatile`的作用是什么？
* 注解 Annotation
    * 生命周期有哪些？
    * 作用对象有哪些？
* 不应该被序列化的字段用什么关键字标记？- `transient`
* WeakReference和WeakReference的区别
* synchronized的用法（类和对象）
* 为什么内部类会持有外部类的引用？
* 怎样继承一个内部类？
* 内部类的构造方法是什么？
* Java中try catch finally的执行顺序？
* Retrofit中如何使用了动态代理？
* RxJava的线程模型是什么？

### FP
* 闭包是什么？lambda是一个闭包吗？
* FP的有哪些特性？
* Reactive 用到了FP的哪些特性？

### 参考资料
* http://blog.csdn.net/crazy__chen/article/details/44901629
* http://www.androidchina.net/6474.html
