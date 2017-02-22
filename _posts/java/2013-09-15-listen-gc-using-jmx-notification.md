---
layout: post
catalog: true
title: "利用JMX的Notifications监听GC"
description: "Notification，GarbageCollectionNotificationInfo"
category: java
subhead: ''
tags: [java, jvm, gc, G1]

---

在Java 7 update 4之前，用java代码监听GC运行情况是不可能的，Java 7 update 4之后，每个垃圾收集器都提供了通知机制，通过监听GarbageCollectorMXBean，可以得到GC完成之后的详细信息。

#### 用法：

      Notification notif;

      // receive the notification emitted by a GarbageCollectorMXBean and set to notif
      ...

      String notifType = notif.getType();
      if (notifType.equals(GarbageCollectionNotificationInfo.GARBAGE_COLLECTION_NOTIFICATION)) {
          // retrieve the garbage collection notification information
          CompositeData cd = (CompositeData) notif.getUserData();
          GarbageCollectionNotificationInfo info = GarbageCollectionNotificationInfo.from(cd);
          ....
      }

在GarbageCollectionNotificationInfo中，能获得大多数GC日志里面的内存：


 |  属性名 | 类型 | 注释 | 
 | ------ | :---- | :---- | | gcName | java.lang.String | 如:G1 Old Generation,G1 Young Generation等 |  | gcAction | java.lang.String | 标识是哪个gc动作，一般为：end of major GC，Young Gen GC等，分别表示老年代和新生代的gc结束。 |  | gcCause | java.lang.String | 引起gc的原因,如：System.gc()，Allocation Failure，G1 Humongous Allocation等 |  | gcInfo | javax.management.openmbean.CompositeData | gc的详细信息，见下表 | 


#### GC详细信息：

 | 属性名 | 类型 | 注释 |
 | ----- | ---- | :---- |  | index | java.lang.Long | 标识这个收集器进行了几次gc |  | startTime | java.lang.Long | gc的开始时间 |  | endTime | java.lang.Long | gc的结束时间 |  | memoryUsageBeforeGc | javax.management.openmbean.TabularData | gc前内存情况 |  | memoryUsageAfterGc | javax.management.openmbean.TabularData | gc后内存情况 | 

#### 内存使用情况：

 | name | desc |
 | --------- | :--------- |
 | init | represents the initial amount of memory (in bytes) that the Java virtual machine requests from the operating system for memory management during startup. The Java virtual machine may request additional memory from the operating system and may also release memory to the system over time. The value of init may be undefined. |  | used | represents the amount of memory currently used (in bytes). |  | committed | represents the amount of memory (in bytes) that is guaranteed to be available for use by the Java virtual machine. The amount of committed memory may change over time (increase or decrease). The Java virtual machine may release memory to the system and committed could be less than init. committed will always be greater than or equal to used. |  | max | represents the maximum amount of memory (in bytes) that can be used for memory management. Its value may be undefined. The maximum amount of memory may change over time if defined. The amount of used and committed memory will always be less than or equal to max if max is defined. A memory allocation may fail if it attempts to increase the used memory such that used > committed even if used less equal max would still be true (for example, when the system is low on virtual memory). | 
   
    
Below is a picture showing an example of a memory pool:


        +----------------------------------------------+
        +////////////////           |                  +
        +////////////////           |                  +
        +----------------------------------------------+

        |--------|
           init
        |---------------|
               used
        |---------------------------|
                  committed
        |----------------------------------------------|    



目前的监听机制只能得到`GC完成之后`的数据，其它环节的GC情况无法观察到。

并且gc的详细信息中的耗时也没有细分`停顿时间`和`并发时间`，相信这个数据更为重要。
        
#### 实例：

<script src="https://gist.github.com/lichengwu/6567273.js"></script>        

        
   


