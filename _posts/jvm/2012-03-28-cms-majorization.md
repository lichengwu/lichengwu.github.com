---
layout: post
title: "一些参数，弥补CMS(Concurrent Mark-Sweep)收集器的缺点"
description: ""
category: jvm
subhead: ''
tags: [jvm, cms, gc]

---

<span style="color: red">参数根据具体应用设置，不是有参数就好。</span>

#### 1.关于碎片问题：


CMS采用Mark-Sweep算法进行垃圾会后，不会对堆空间进行整理和压缩，每次回收后不可避免会有一些碎片产生。

**-XX:+UseCMSCompactAtFullCollection**   default true 对老年代进行压缩，可能影响性能，但是可以消除碎片。

**-XX:CMSFullGCsBeforeCompaction=n** CMS进行n次full gc后进行一次压缩。<span style="color: red">如果n=0,每次full gc后都会进行碎片压缩。</span>

#### 2.关于CPU问题

CMS需要CPU有更多的“核”，在CMS活动的时候，也会占用较多的“核”。

**–XX:+CMSIncrementalMode**   default false 并发收集递增进行，周期性把cpu资源让给正在运行的应用。

**–XX:+CMSIncrementalPacing**  default false 根据应用程序的行为自动调整每次执行的垃圾回收任务的数量

**–XX:ParallelGCThreads=n**   (ncpus <= 8) ? ncpus : 3 + ((ncpus * 5) / 8) 并发垃圾回收线程数

**-XX:CMSIncrementalDutyCycleMin=n**  default 0  每次增量回收垃圾的占总垃圾回收任务的最小比例

**-XX:CMSIncrementalDutyCycle=n**  default 10 每次增量回收垃圾的占总垃圾回收任务的比例

#### 3.关于空间问题

CMS需要更多的内存，一方面是碎片问题，另一方面是对老年代回收不是在老年代满的时候，而是当老年代内存达到一定比例。

**-XX:CMSInitiatingOccupancyFractio=n**  当老年代内存使用达到n%,开始会后 jdk5 默认是`68%` jdk6默认`92%` CMSInitiatingOccupancyFraction = (100 - MinHeapFreeRatio) + (CMSTriggerRatio * MinHeapFreeRatio / 100)


