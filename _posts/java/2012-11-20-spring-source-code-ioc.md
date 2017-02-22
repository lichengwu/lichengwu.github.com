---
layout: post
catalog: true
title: "Spring源码阅读——Ioc初始化过程"
description: ""
category: spring
subhead: ''
tags: [spring,  ioc,  java, source]

---

以web项目启动为例，介绍一下Ioc容器的初始化。

下面这个图主要是在启动项目的时候，跟踪代码所得到的，不同的配置可能会有不同的路径，但是图中勾勒出了必须经历的大部分过程。

首先，在web.xml中配置ContextLoaderListener，当启动项目的会有下图：

![image](/images/java/ioc_zps75bb1a82.png)

#### Ioc初始化大概分三个步骤：
 
##### 准备工作(黑线)

##### 加载Resource(红线)

##### 通过Resource加载Bean定义并注册到Ioc容器中(绿线)


