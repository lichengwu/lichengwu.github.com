---
layout: post
title: "利用JMX的Notifications监听GC"
description: ""
category: java
subhead: ''
tags: [java, jvm, gc, G1]

---

在Java 7 update 4之前，用java代码监听GC运行情况是不可能的，Java 7 update 4之后，每个垃圾收集器都提供了通知机制，通过监听GarbageCollectorMXBean，可以得到GC完成之后的详细信息。

####用法：

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


<table class="table table-bordered table-striped table-condensed">
   <tr>
      <th>属性名</th>
      <th>类型</th>
      <th>注释</th>
   </tr>
   <tr>
      <td>gcName</td>
      <td>java.lang.String</td>
      <td>如:G1 Old Generation,G1 Young Generation等</td>
   </tr>
   <tr>
      <td>gcAction</td>
      <td>java.lang.String</td>
      <td>标识是哪个gc动作，一般为：end of major GC，Young Gen GC等，分别表示老年代和新生代的gc结束。</td>
   </tr>
   <tr>
      <td>gcCause</td>
      <td>java.lang.String</td>
      <td>引起gc的原因,如：System.gc()，Allocation Failure，G1 Humongous Allocation等</td>
   </tr>
   <tr>
      <td>gcInfo</td>
      <td>javax.management.openmbean.CompositeData</td>
      <td>gc的详细信息，见下表</td>
   </tr>
</table>

####GC详细信息：

<table class="table table-bordered table-striped table-condensed">
   <tr>
      <th>属性名</th>
      <th>类型</th>
      <th>注释</th>
   </tr>
   <tr>
      <td>index</td>
      <td>java.lang.Long</td>
      <td>标识这个收集器进行了几次gc</td>
   </tr>
   <tr>
      <td>startTime</td>
      <td>java.lang.Long</td>
      <td>gc的开始时间</td>
   </tr>
   <tr>
      <td>endTime</td>
      <td>java.lang.Long</td>
      <td>gc的结束时间</td>
   </tr>
   <tr>
      <td>memoryUsageBeforeGc</td>
      <td>javax.management.openmbean.TabularData</td>
      <td>gc前内存情况</td>
   </tr>
   <tr>
      <td>memoryUsageAfterGc</td>
      <td>javax.management.openmbean.TabularData</td>
      <td>gc后内存情况</td>
   </tr>
</table>

####内存使用情况：

<table class="table table-bordered table-striped table-condensed">
     <tr>
      <td>init</td>
      <td>represents the initial amount of memory (in bytes) that the Java virtual machine requests from the operating system for memory management during startup. The Java virtual machine may request additional memory from the operating system and may also release memory to the system over time. The value of init may be undefined.
      </td>
    </tr>
    <tr>
        <td>used</td>
        <td>represents the amount of memory currently used (in bytes).
        </td>
    </tr>
    <tr>
        <td>committed</td>
        <td>represents the amount of memory (in bytes) that is guaranteed to be available for use by the Java virtual machine. The amount of committed memory may change over time (increase or decrease). The Java virtual machine may release memory to the system and committed could be less than init. committed will always be greater than or equal to used.
        </td>
    </tr>
    <tr>
        <td>max</td>
        <td>
        represents the maximum amount of memory (in bytes) that can be used for memory management. Its value may be undefined. The maximum amount of memory may change over time if defined. The amount of used and committed memory will always be less than or equal to max if max is defined. A memory allocation may fail if it attempts to increase the used memory such that used > committed even if used less equal max would still be true (for example, when the system is low on virtual memory).
        </td>
    </tr>
</table> 
   
    
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



目前的监听机制只能，得到`GC完成之后`的数据，其它环节的GC情况无法观察到。

并且gc的详细信息中的耗时也没有细分`停顿时间`和`并发时间`，相信这个数据更为重要。
        
####实例：

<script src="https://gist.github.com/lichengwu/6567273.js"></script>        

        
   


{% include JB/setup %}