---
title: Anko的设计之道
date: 2018-05-17 19:58:05
tags:
 - kotlin
 - anko
---

Anko 是一个完全基于 Kotlin 设计的 Android 三方库，命名来自于 (An)droid (Ko)tlin。
Anko 试图建立一套新的 Android 开发范式，虽然不会成为主流，但是它的设计思想值得我们借鉴。

## 新的 UI 体系

先看一下 Anko 用于构建 UI 的几个关键类：

    +--------------+
    | ViewManaager |
    +-------.------+
           /|\
            |
    +---------------+
    |  AnkoContext  |<--------------+
    +-------.-------+               |
           /|\                      |
            |                       |
    +-----------------+   +-----------------------+
    | AnkoContextImpl |   | DelegatingAnkoContext |
    +-------.---------+   +-----------------------+
           /|\
            |
    +---------------------+
    | ReusableContextImpl |
    +---------------------+

首先需要说明，`AnkoContext` 继承自 `ViewManager`，而不是 `android.content.Context`。Anko 创建 UI 组件也需要基于 `AnkoContext`，就好像 Android 需要基于 `Context` 来创建一个 `View`。

`AnkoContext` 与 `android.content.Context` 的地位是并列的，大部分为 `Context` 定义扩展的地方也定义了 `AnkoContext` 的扩展，例如 `Dimensions.kt` 文件：

```kotlin
fun Context.dip(value: Int): Int = (value * resources.displayMetrics.density).toInt()
inline fun AnkoContext<*>.dip(value: Int): Int = ctx.dip(value)
inline fun View.dip(value: Int): Int = context.dip(value)
inline fun Fragment.dip(value: Int): Int = activity.dip(value)
```

Anko 的 UI 组件用一个接口 `AnkoComponent` 来表示，接口内提供了一个模板方法 `createView`：

```kotlin
interface AnkoComponent<in T> {
    fun createView(ui: AnkoContext<T>): View
}
```

`AnkoComponent` 的类型参数是一个逆变的声明处变型，模板方法 `createView` 的作用是基于 `AnkoContext<T>` 实例创建一个 `View` 实例。

`AnkoContext` 是一个继承自 `ViewManager` 的接口，它“内藏”了 Android 的 `Context` 实例，同时还定义了一个类型为 `T` 的属性 `owner`，表示 `AnkoContext` 的拥有者，`AnkoContext` 没有对类型参数 `T` 加以限定，任何类型都可以。

```kotlin
interface AnkoContext<out T> : ViewManager {
  val ctx: Context
  val owner: T
  val view: View

  override fun updateViewLayout(view: View, params: ViewGroup.LayoutParams) {
    throw UnsupportedOperationException()
  }

  override fun removeView(view: View) {
    throw UnsupportedOperationException()
  }

  companion object {
    fun create(ctx: Context, setContentView: Boolean = false): AnkoContext<Context>
          = AnkoContextImpl(ctx, ctx, setContentView)

    fun createReusable(ctx: Context, setContentView: Boolean = false): AnkoContext<Context>
          = ReusableAnkoContext(ctx, ctx, setContentView)

    fun <T> create(ctx: Context, owner: T, setContentView: Boolean = false): AnkoContext<T>
          = AnkoContextImpl(ctx, owner, setContentView)

    fun <T> createReusable(ctx: Context, owner: T, setContentView: Boolean = false): AnkoContext<T>
          = ReusableAnkoContext(ctx, owner, setContentView)

    fun <T: ViewGroup> createDelegate(owner: T): AnkoContext<T> = DelegatingAnkoContext(owner)
  }
}
```

`AnkoContext` 还通过伴生对象（companion object）提供了几个工厂方法，它们生产出来的对象都是 `AnkoContext` 的子类，上面的类关系图已经展示出了它们之间的关系。

`AnkoContextImpl` 是 `AnkoContext` 接口的具体实现，覆写了 `ViewManager#addView` 方法：

```kotlin
override fun addView(view: View?, params: ViewGroup.LayoutParams?) {
  if (view == null) return

  if (myView != null) {
    alreadyHasView()
  }

  this.myView = view

  if (setContentView) {
    doAddView(ctx, view)
  }
}
```

我们来分析一下 `addView` 的具体实现：

参数 `view: View?` 是一个可空类型，但是 `AnkoContext` 的属性 `view: View` 是一个非可空类型，所以 `AnkoContextImpl` 重新定义了一个属性 `var myView: View?` 用于存储参数 `view`：

```kotlin
private var myView: View? = null

override val view: View
  get() = myView ?: throw IllegalStateException("View was not set previously")
```

`AnkoContextImpl` 的构造方法多了一个属性参数 `setContentView: Boolean`，它表示是否要把 `addView` 的参数 `view` 设置为属性 `ctx: Context` 的 content view：

```kotlin
  private fun doAddView(context: Context, view: View) {
    when (context) {
        is Activity -> context.setContentView(view)
        is ContextWrapper -> doAddView(context.baseContext, view)
        else -> throw IllegalStateException("Context is not an Activity, can't set content view")
    }
  }
```

`ReusableAnkoContext` 继承自 `AnkoContextImpl`，两者唯一的不同点在于 `ReusableAnkoContext` 的 `alreadHasView` 是一个空实现，而 `AnkoContextImpl` 会抛出一个异常，它不支持 view 复用。

`DelegatingAnkoContext` 是 `AnkoContext` 的另一个实现：

```kotlin
internal class DelegatingAnkoContext<T: ViewGroup>(override val owner: T): AnkoContext<T> {
  override val ctx: Context = owner.context
  override val view: View = owner

  override fun addView(view: View?, params: ViewGroup.LayoutParams?) {
      if (view == null) return

      if (params == null) {
          owner.addView(view)
      } else {
          owner.addView(view, params)
      }
  }
}
```

`DelegatingAnkoContext` 的类型参数指定了 upper bound —— `<T: ViewGroup>`，也就是说 `DelegatingAnkoContext` 的 `owner` 必须是 `ViewGroup` 或者它的子类。

关于 `DelegatingAnkoContext` 名字中的 `Delegating` 应该是为了表达属性 `view: View` 代理了（delegating）`owner: T`：

```kotlin
override val view: View = owner
```

再来回看一下这几个类的继承关系：

    +--------------+
    | ViewManaager |
    +-------.------+
           /|\
            |
    +---------------+
    |  AnkoContext  |<--------------+
    +-------.-------+               |
           /|\                      |
            |                       |
    +-----------------+   +-----------------------+
    | AnkoContextImpl |   | DelegatingAnkoContext |
    +-------.---------+   +-----------------------+
           /|\
            |
    +---------------------+
    | ReusableContextImpl |
    +---------------------+


## 使用扩展增加新功能

扩展（extension）在完全不影响原有类（Receiver）的前提下带来了新的功能，例如下面的示例代码：

* 为 `Array` 增加一个 `forEachByIndex` 的扩展函数：

```kotlin
inline fun <T> Array<T>.forEachByIndex(f: (T) -> Unit) {
    val lastIndex = size - 1
    for (i in 0..lastIndex) {
        f(get(i))
    }
}
```

* 为 `ViewGroup` 增加一个 `forEachChild` 的扩展函数：

```kotlin
inline fun ViewGroup.forEachChild(action: (View) -> Unit) {
    for (i in 0..childCount - 1) {
        action(getChildAt(i))
    }
}
```

这类比较短小的扩展一般会定义为**内联函数**（inline function），所以完全没必要担心函数调用所带来的性能开销。基于扩展，可以让一些比较“老旧”的类重新焕发出春天。

扩展还可以很轻松地支持链式调用，例如下面的扩展函数借助 Koltin 的标准函数 `apply`，不但让函数定义自身变得非常简洁，而且 `addFlags` 之后还能返回 `Intent` 本身，完美支持链式调用。

```kotlin
inline fun Intent.clearTask(): Intent = apply { addFlags(Intent.FLAG_ACTIVITY_CLEAR_TASK) }
inline fun Intent.clearTop(): Intent = apply { addFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP) }
inline fun Intent.multipleTask(): Intent = apply { addFlags(Intent.FLAG_ACTIVITY_MULTIPLE_TASK) }
inline fun Intent.newTask(): Intent = apply { addFlags(Intent.FLAG_ACTIVITY_NEW_TASK) }
inline fun Intent.singleTop(): Intent = apply { addFlags(Intent.FLAG_ACTIVITY_SINGLE_TOP) }
```

## 基于 DSL 实现 UI 布局

DSL来取代手写 XML 布局的优势是：类型安全、代码复用、性能提升。

先来看一段代码：

```kotlin
verticalLayout {
    padding = dip(32)

    imageView(android.R.drawable.ic_menu_manage).lparams {
        margin = dip(16)
        gravity = Gravity.CENTER
    }

    val name = editText {
        hintResource = R.string.name
    }
    val password = editText {
        hintResource = R.string.password
        inputType = TYPE_CLASS_TEXT or TYPE_TEXT_VARIATION_PASSWORD
    }

    button("Log in") {
        onClick {
            ui.owner.tryLogin(ui, name.text, password.text)
        }
    }

    myRichView()
}.applyRecursively(customStyle)
```

Anko 新的 UI 体系中，`AnkoComponent` 来表示一个 UI 组件，`createView` 返回 `View` 示例，上例中的代码即可作为 `createView` 的函数内容，返回值是一个 `VerticalLayout`。

我们来继续分析 UI DSL 的原理

> to be continued

