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
      <th>默认值</th>
      <th>范围</th>
      <th>适用版本</th>
      <th>备注</th>
   </tr>
   <tr>
      <td>-XX:+UseConcMarkSweepGC</td>
      <td>false</td>
      <td>[true,false]</td>
      <td></td>
      <td>老年代采用CMS收集器收集</td>
   </tr>
   <tr>
      <td>-XX:+CMSScavengeBeforeRemark</td>
      <td>false</td>
      <td>[true,false]</td>
      <td></td>
      <td>The CMSScavengeBeforeRemark forces scavenge invocation from the CMS-remark phase (from within the VM thread as the CMS-remark operation is executed in the foreground collector).</td>
   </tr>
   <tr>
      <td>-XX:+UseCMSCompactAtFullCollection</td>
      <td>false</td>
      <td>[true,false]</td>
      <td></td>
      <td>对老年代进行压缩，可以消除碎片，但是可能会带来性能消耗</td>
   </tr>
   <tr>
      <td>-XX:CMSFullGCsBeforeCompaction=n</td>
      <td>0</td>
      <td>[0,～]</td>
      <td></td>
      <td>CMS进行n次full gc后进行一次压缩。如果n=0,每次full gc后都会进行碎片压缩。如果n=0,每次full gc后都会进行碎片压缩</td>
   </tr>
</table>


{% include JB/setup %}
