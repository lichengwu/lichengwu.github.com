---
layout: post
catalog: true
title: "JVM资料汇总"
description: "JVM资料汇总;GC资料汇总"
category: jvm
subhead: ''
tags: [jvm, java, gc]

---

基础知识
--------

1.  java内存管理白皮书[http://www.oracle.com/technetwork/java/javase/memorymanagement-whitepaper-150215.pdf](http://www.oracle.com/technetwork/java/javase/memorymanagement-whitepaper-150215.pdf)
2.  开发者工具 [http://docs.oracle.com/javase/7/docs/technotes/tools/](http://docs.oracle.com/javase/7/docs/technotes/tools/)

参数
----

1.  中文版java6 JVM参数：[http://kenwublog.com/docs/java6-jvm-options-chinese-edition.htm](http://kenwublog.com/docs/java6-jvm-options-chinese-edition.htm)
2.  官方JVM参数说明：[http://www.oracle.com/technetwork/java/javase/tech/vmoptions-jsp-140102.html](http://www.oracle.com/technetwork/java/javase/tech/vmoptions-jsp-140102.html)
3.  关于CMS的几个参数： [http://blog.lichengwu.cn/jvm/2013/10/14/jvm-command-line-options/](http://blog.lichengwu.cn/jvm/2013/10/14/jvm-command-line-options/)

GC相关
------

### CMS

1.  理解CMS日志：[https://blogs.oracle.com/poonam/entry/understanding_cms_gc_logs](https://blogs.oracle.com/poonam/entry/understanding_cms_gc_logs)
2.  Concurrent Mode精辟解释，同时也是官方版：[https://blogs.oracle.com/jonthecollector/entry/what_the_heck_s_a](https://blogs.oracle.com/jonthecollector/entry/what_the_heck_s_a)
3.  CMS GC背后一些技术细节：[https://blogs.oracle.com/jonthecollector/entry/did_you_know](https://blogs.oracle.com/jonthecollector/entry/did_you_know)

### G1

1.  G1论文，G1的模型好早就提出来了，最近才真正实现。[http://www.oracle.com/technetwork/server-storage/ts-5419-159484.pdf](http://www.oracle.com/technetwork/server-storage/ts-5419-159484.pdf)
2.  官方G1教程\[原理，与CMS对比\][http://www.oracle.com/webfolder/technetwork/tutorials/obe/java/G1GettingStarted/index.html](http://www.oracle.com/webfolder/technetwork/tutorials/obe/java/G1GettingStarted/index.html)
3.  官方G1介绍 [http://www.oracle.com/technetwork/java/javase/tech/g1-intro-jsp-135488.html](http://www.oracle.com/technetwork/java/javase/tech/g1-intro-jsp-135488.html)
4.  理解G1日志 [https://blogs.oracle.com/poonam/entry/understanding_g1_gc_logs](https://blogs.oracle.com/poonam/entry/understanding_g1_gc_logs)

build OopenJDK
--------------

官方说明，如何编译OpenJDK：[http://hg.openjdk.java.net/jdk7/build/raw-file/tip/README-builds.html](http://hg.openjdk.java.net/jdk7/build/raw-file/tip/README-builds.html)

### Windows

Windows上编译OpenJDK难度比较大，主要是依赖太多，配置编译环境麻烦。

这里有个例子，有兴趣可以研究一下：[http://blog.csdn.net/gnefniu/article/details/7515394](http://blog.csdn.net/gnefniu/article/details/7515394)

### Linux

linux编译OpenJDK比较简单：[http://blog.lichengwu.cn/jvm/2012/06/13/compile-openjdk7-on-ubuntu12-04/](http://blog.lichengwu.cn/jvm/2012/06/13/compile-openjdk7-on-ubuntu12-04/)

这里还有个内网的文章，编译&debug JVM：[http://www.atatech.org/article/detail/8400/97](http://www.atatech.org/article/detail/8400/97)

监控相关
--------

1.  利用VisualVM监视远程JVM: [http://blog.lichengwu.cn/jvm/2011/11/17/visual-mv-monitoring-jvm/](http://blog.lichengwu.cn/jvm/2011/11/17/visual-mv-monitoring-jvm/)
2.  利用JMX的Notifications监听GC:[http://blog.lichengwu.cn/java/2013/09/15/listen-gc-using-jmx-notification/](http://blog.lichengwu.cn/java/2013/09/15/listen-gc-using-jmx-notification/)
3.  JMX监控\[官方\]：[http://docs.oracle.com/javase/1.5.0/docs/guide/management/agent.html](http://docs.oracle.com/javase/1.5.0/docs/guide/management/agent.html)

源码阅读
--------

1.  HotSpot VM源码结构：[http://blog.lichengwu.cn/jvm/2013/07/10/hotspot-vm-source-structure/](http://blog.lichengwu.cn/jvm/2013/07/10/hotspot-vm-source-structure/)
2.  OpenJDK源码阅读导航，撒迦的博客： [http://rednaxelafx.iteye.com/blog/1549577](http://rednaxelafx.iteye.com/blog/1549577)
3.  OpenJDK源码地址： [http://openjdk.java.net/](http://openjdk.java.net/)

技术优化
--------

1.  从JVM看Synchronization 和 Object Locking\[官方\] ：[https://wikis.oracle.com/display/HotSpotInternals/Synchronization](https://wikis.oracle.com/display/HotSpotInternals/Synchronization)
2.  偏向锁是JVM内部一个小优化，因为一个CAS原语在JVM看来还是比较浪费的，JVM会把锁偏向于第一个获得锁的线程，节省锁开销：[https://blogs.oracle.com/dave/entry/biased_locking_in_hotspot](https://blogs.oracle.com/dave/entry/biased_locking_in_hotspot)
3.  fork-join JDK7增强了fork-join模型支持，这个未来，先了解一下吧：[http://www.ibm.com/developerworks/cn/java/j-jtp11137.html](http://www.ibm.com/developerworks/cn/java/j-jtp11137.html)
4.  栈上替换(on stack replacement),JVM运行时优化： [http://xmlandmore.blogspot.com/2012/06/on-stack-replacement-in-hotspot-jvm.html](http://xmlandmore.blogspot.com/2012/06/on-stack-replacement-in-hotspot-jvm.html)

其它资源
--------

### 博客

1.  Jon Masamitsu(JDK核心开发成员，专注垃圾收集)：[https://blogs.oracle.com/jonthecollector/](https://blogs.oracle.com/jonthecollector/)
2.  国内氛围比较好的虚拟机讨论社区：[http://hllvm.group.iteye.com/](http://hllvm.group.iteye.com/)
3.  撒迦的博客：[http://rednaxelafx.iteye.com/](http://rednaxelafx.iteye.com/)

### 推荐书籍

1.  《Java虚拟机规范》：[http://icyfenix.iteye.com/blog/1256329](http://icyfenix.iteye.com/blog/1256329) 推荐看英文版：[http://docs.oracle.com/javase/specs/index.html](http://docs.oracle.com/javase/specs/index.html) 这里有html，pdf下载
2.  《Java Performance》（一直想读，推荐一下，非常不错，这么好的一本书，没人翻译）： [http://book.douban.com/subject/5980062/](http://book.douban.com/subject/5980062/)
3.  《深入理解Java虚拟机》，有人说这本书太浅了，但是要看你怎么阅读。首先这本书从整理上介绍了JVM的组成，每一章都是一个值得研究的方向，在好的书也无法涵盖所有内容，剩下的就得靠自己去探索 [http://book.douban.com/subject/24722612/](http://book.douban.com/subject/24722612/)
4.  《深入理解计算机系统》，国外经典教材，基础不牢固，很难深入研究JVM：[http://book.douban.com/subject/5407246/](http://book.douban.com/subject/5407246/)
5.  《深入Java虚拟机》，失传江湖的最早将JVM的书，已经没有出版了，不过可以下载PDF，里面的内容是基于JDK1.2以前讲的，有些技术已经过时了，不过我们可以拿来当历史书看啊！[http://book.douban.com/subject/1138768/](http://book.douban.com/subject/1138768/)
6.  《虚拟机》经典虚拟机理论书籍，通用虚拟机概述，设计软件和硬件，需要一定基础哇：[http://book.douban.com/subject/3611865/](http://book.douban.com/subject/3611865/)

