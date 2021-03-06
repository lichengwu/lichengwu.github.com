---
layout: post
catalog: true
title: "《敏捷软件开发》读书笔记"
description: ""
category: design-pattern
subhead: ''
tags: [java, design-pattern, agile]

---
零零散散，利用下班和周末时间看完了大师之作。记录下自己的笔记，做个积累。

----

## 关于敏捷开发

合作，沟通以及交互能力要比单纯的编程能力更为重要。

合适的工具对于成功来说是非常重要的。

直到迫切需要并且意义重大时，才编制文档。

成功的项目需要有序、频繁的客户反馈。

较好的做策划的策略是：为下两周做详细计划，为下三个月做粗略计划，再以后就做极为粗糙的计划。

敏捷实践会尽早地，经常地进行交付。

当你能够度量你所说的，并且能够用数组表达它时，就表示你了解了它；若你不能度量它，不能用数组去表达它，那么说明你的知识就是匮乏的、不能令人满意的。

----

## 关于测试

程序中的每一项功能都有测试来验证它的操作的正确性。

首先编写测试可以迫使我们使用不同的观察点。

测试可以作为一种无价的文档形式。

测试的好处：验收、文档、架构。

----

## 关于重构

不要放弃任何一次重构的机会。

每天清洁你的代码。

----

## 面向对象设计原则

**SRP 单一职责原则**

就一个类而言，应该仅有一个引起它变化的原因。

**OCP 开放——封闭原则**

软件实体（类、模块、函数等）应该是可以扩展的，但是不可修改。 

**LSP Liskov替换原则**

子类型必须能够替换掉它们的基类型。

**DIP 依赖倒置原则**

高层模块不应该依赖于底层模块。二者都应该依赖于抽象。

抽象不应该依赖于细节，细节应该依赖于抽象。

每个较高层次都为它所需要的服务声明一个抽象接口，较低的层次实现了这些抽象接口，每个高层类都通过该抽象接口使用下一层，这样高层就不依赖于底层。

底层模块实现了高层模块中声明并被高层模块调用的接口。

**ISP 接口隔离原则**

不应该强迫客户依赖于它们不用的方法。接口属于客户，不属于它所在的类层次结构。

----

## 包的设计原则

**REP 重用发布等价原则**

重用的粒度就是发布的粒度。

**CCP 共同封闭原则**

包中的所有类对于同一类性质的变化应该是共同封闭的。一个变化若对一个包产生影响，则将对改包中的所有类都产生影响，而对于其他的包不造成影响。

**CRP 共同重用原则**

一个包中的所用类应该是共同重用的。如果重用了包中的一个类，那么就要重用包中的所有类。

**ADP 无环依赖原则**

在包中的依赖关系图中不允许存在环。

**SDP 稳定依赖原则**

朝着稳定的方向进行依赖。

**SAP 稳定抽象原则**

包的抽象程度应该和其稳定程度一致。

---

## 设计模式

大多数类都是一组方法和相应的一组变量的组合，`COMMAND`模式不是这样的，它只封装了一个没有任何变量的函数。

`TEMPLATE METHOD`模式使用继承来解决问题，而`STRATEGY`模式使用的则是委托。

`FACADE`模式从上面施加策略，而`MEDIATOR`模式则从下面施加策略。

远在真正需要`PROXY`模式或者`STAIRWAY TO　HEAVEN` 模式前，就去预测对于它们的需要是非常有诱惑力的，但是这个不是一个好主意。

`VISITOR`模式系列允许在不更改现有类层次结构的情况下向其中增加新方法。`双重分发`(dual dispatch)技术是VISITOR的核心机制。

如果一个应用程序中存在有需要`以多种不同的方式进行解释的数据结构`，就可以使用VISITOR模式。




