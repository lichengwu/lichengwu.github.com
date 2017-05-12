---
layout: post
catalog: true
title: "Kotlin问题汇总"
description: ""
category: kotlin
subhead: ''
tags: [kotlin,reference]

---

### 与Spring AOP结合

在默认情况下，kotlin定义的非抽象class是final类型，字段也是一样；而在Spring中，AOP是不能代理final的类或者成员变量的，
final的类代理会报异常，而final的成员变量代理后是`null`。目前这是个很隐蔽的坑，有的时候在运行时才能发现。解决方法就是在类或者成员变量上加上`open`修饰。

````
@Service
open class OrderPersistFun {

    @Resource
    open var orderPayAssembleFun: OrderPayAssembleFun? = null

    @Resource
    open var orderManager: OrderManager? = null
}

````
