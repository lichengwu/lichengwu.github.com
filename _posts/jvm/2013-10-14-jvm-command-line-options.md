---
layout: post
title: "JVM参数大全"
description: "常用JVM参数"
category: jvm
subhead: '持续更新'
tags: [java, jvm, hotspot]

---

###CMS相关
<table class="table table-bordered table-striped table-condensed">
   <tr>
      <th>选项</th>
      <th>类型</th>
      <th>默认值</th>
      <th>备注</th>
   </tr>
   <tr>
      <td>-XX:+UseConcMarkSweepGC</td>
      <td>boolean</td>
      <td>false</td>
      <td>老年代采用CMS收集器收集</td>
   </tr>
   <tr>
      <td>-XX:+CMSScavengeBeforeRemark</td>
      <td>boolean</td>
      <td>false</td>
      <td>The CMSScavengeBeforeRemark forces scavenge invocation from the CMS-remark phase (from within the VM thread as the CMS-remark operation is executed in the foreground collector).</td>
   </tr>
   <tr>
      <td>-XX:+UseCMSCompactAtFullCollection</td>
      <td>boolean</td>
      <td>false</td>
      <td>对老年代进行压缩，可以消除碎片，但是可能会带来性能消耗</td>
   </tr>
   <tr>
      <td>-XX:CMSFullGCsBeforeCompaction=n</td>
      <td>uintx</td>
      <td>0</td>
      <td>CMS进行n次full gc后进行一次压缩。如果n=0,每次full gc后都会进行碎片压缩。如果n=0,每次full gc后都会进行碎片压缩</td>
   </tr>
   <tr>
      <td>–XX:+CMSIncrementalMode</td>
      <td>boolean</td>
      <td>false</td>
      <td>并发收集递增进行，周期性把cpu资源让给正在运行的应用</td>
   </tr>
   <tr>
      <td>–XX:+CMSIncrementalPacing</td>
      <td>boolean</td>
      <td>false</td>
      <td>根据应用程序的行为自动调整每次执行的垃圾回收任务的数量</td>
   </tr>
   <tr>
      <td>–XX:ParallelGCThreads=n</td>
      <td>uintx</td>
      <td></td>
      <td>并发回收线程数量：(ncpus &lt;= 8) ? ncpus : 3 + ((ncpus * 5) / 8)</td>
   </tr>
   <tr>
      <td>-XX:CMSIncrementalDutyCycleMin=n</td>
      <td>uintx</td>
      <td>0</td>
      <td>每次增量回收垃圾的占总垃圾回收任务的最小比例</td>
   </tr>
   <tr>
      <td>-XX:CMSIncrementalDutyCycle=n</td>
      <td>uintx</td>
      <td>10</td>
      <td>每次增量回收垃圾的占总垃圾回收任务的比例</td>
   </tr>
   <tr>
      <td>-XX:CMSInitiatingOccupancyFractio=n</td>
      <td>uintx</td>
      <td>jdk5 默认是68% jdk6默认92%</td>
      <td>当老年代内存使用达到n%,开始回收。`CMSInitiatingOccupancyFraction = (100 - MinHeapFreeRatio) + (CMSTriggerRatio * MinHeapFreeRatio / 100)`</td>
   </tr>
   <tr>
      <td>-XX:CMSMaxAbortablePrecleanTime=n</td>
      <td>intx</td>
      <td>5000</td>
      <td>在CMS的preclean阶段开始前，等待minor gc的最大时间。[see here](https://blogs.oracle.com/jonthecollector/entry/did_you_know) </td>
   </tr>
</table>


{% include JB/setup %}
