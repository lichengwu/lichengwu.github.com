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
      <td>–XX:+CMSScavengeBeforeRemark</td>
      <td>false</td>
      <td>\[true,false\]</td>
      <td>1.4+</td>
      <td>The CMSScavengeBeforeRemark forces scavenge invocation from the CMS-remark phase (from within the VM thread as the CMS-remark operation is executed in the foreground collector).</td>
   </tr>
</table>


{% include JB/setup %}
