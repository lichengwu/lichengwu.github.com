---
layout: post
title: "Garbage First(G1)介绍"
description: ""
category: jvm
subhead: ''
tags: [jvm, java, G1]

---

#### 介绍：

Garbage First(G1)致力于在多CPU和大内存服务器上对垃圾收集提供软实时目标(soft real-time goal )和高吞吐量(high throughput )。从JDK 6u14开始就已经在Hotspot上试验，到现在的DK7依然没有走出实验室：
  
    #java -version
    java version "1.7.0_03"
    Java(TM) SE Runtime Environment (build 1.7.0_03-b05)
    Java HotSpot(TM) 64-Bit Server VM (build 22.1-b02, mixed mode)
  
    #java -server -XX:+PrintFlagsFinal | grep UseG1GC
    bool UseG1GC                                   = false           {product}
 
#### 关于region：

在G1中，heap被平均分成若干个大小相等的区域(region)。每个region都有一个关联的remembered set (RS)，RS的数据结构是hash table，里面的数据是card table (heap中每512byte映射在card table 1byte)。简单的说RS里面存在的是region中live objects的指针。当region中数据发生变化时，首先反映到card table中的一个或多个card上，RS通过扫描内部的card table得知region中内存使用情况和存活对象。

#### 关于内存分配：

由于G1主要关注于多CPU多线程，所以内存分配采用 thread-local allocation buffers (TLABs)技术。每个分配线程都有一个自己的buffers用来分配对象，当buffers用完或者不够的时候，去重新申请一块内存放在自己的thread-local里面。这样对象的内存分配被最小化到私有的buffers里面，缓解了并发分配内存的压力。

当region被填满后，分配内存的线程会重新选择一个新的region。空region被组织到一个linked list里面，这样可以快速找到新的region。

对于大对象的分配不是在TLABs进行的，而是在TLABs之外。当一个对象的大小超过region的3/4的时候，这个对象被认为是巨大的(humongous )。巨大的对象被分配到特殊的区域(heap regions )。这些区域只包含巨大对象(humongous object )。

#### 执行过程：

* 初始标记 :Initial Marking
* 并发标记 :Concurrent Marking
* 最终标记 :Final Marking
* 计数并清理 :Live Data Counting and Cleanup

G1执行的第一阶段是初始标记(Initial Marking ),这个阶段是STW(Stop the World )的，所有mutator threads将被停止，标记出从GC Root开始直接可达的对象。然后，所有mutator threads将被重启，进入并发标记(Concurrent Marking )阶段。这个阶段从GC Root开始对heap中的对象标记，标记线程与应用程序线程并行直接，耗时较长。当并发标记完成后，开始最终标记(Final Marking )阶段。这个阶段主要是标记那些在并发标记阶段发生变化的对象。同样最终标记也要STW，但是多个标记线程并行运行，很快就可以完成。最后一个阶段会对每个区域(region)的回收成本和价值进行排序，根据用户指定的停顿时间，选择性的收集某些区域的对象，并统计每个区域对象的数量。

##### 与CMS对比：

总体来说，G1跟CMS一样，是一块低延时的收集器，同样牺牲了吞吐量，不过二者之间得到了很好的权衡。
G1与CMS对比有一下不同：

**1.分代：** CMS中，堆被分为PermGen，YoungGen，OldGen；而YoungGen又分了两个survivo区域。在G1中，堆被平均分成几个区域(region)，在每个区域中，虽然也保留了新老代的概念，但是收集器是以整个区域为单位收集的。

**2.算法：** 相对于CMS的“标记——清理”算法，G1会使用压缩算法，保证不产生多余的碎片。收集阶段，G1会将某个区域存活的对象拷贝的其他区域，然后将整个区域整个回收。

**3.停顿时间可控：** 为了缩短停顿时间，G1建立可预存停顿模型，这样在用户设置的停顿时间范围内，G1会选择适当的区域进行收集，确保停顿时间不超过用户指定时间。

{% include JB/setup %}
