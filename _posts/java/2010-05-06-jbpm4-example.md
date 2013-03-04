---
layout: post
title: "jBPM4.3 一个请假例子 web"
description: ""
category: java
subhead: ''
tags: [jbpm,  java]

---

这个例子不能说是完全原创，是在一个例子的基础上修改的，不过拿出来分享大家请轻砸。

jbpm的例子不是很多，而且前篇一律。高级的东西还得看开发手册跟源码。

不多说，上图:

![](http://i1298.photobucket.com/albums/ag53/lichengwu/1_zps9e315d23.gif)

对应的source


    <?xml version="1.0" encoding="UTF-8"?> 
    <process name="loan" xmlns="http://jbpm.org/4.3/jpdl"> 
       <start g="147,21,48,48" name="start"> 
          <transition g="8,-9" name="提出申请" to="请假申请"/> 
       </start> 
       <end g="39,442,48,48" name="end"/> 
       <task assignee="#{user}" form="request.jsp" g="124,122,92,52" name="请假申请"> 
          <transition g="11,-10" name="to_teacher" to="班主任审批"/> 
      </task> 
      <task assignee="teacher" form="request_teacher.jsp" g="125,218,92,52" name="班主任审批"> 
         <transition g="4,-10" name="批准" to="exclusive1"/> 
         <transition name="驳回" to="cancel" g="436,263:-59,-17"/> 
      </task> 
      <task assignee="director" form="request_director.jsp" g="313,326,92,52" name="年级主任审批"> 
        <transition g="238,467:-24,-24" name="批准" to="end"/> 
        <transition name="驳回" to="cancel" g="3,-12"/> 
      </task> 
     <decision expr="#{days >= 3 ? 'to_director' : '批准'}" g="149,317,48,48" name="exclusive1"> 
     <transition g="-34,-20" name="to_director" to="年级主任审批"/> 
         <transition g="-40,-16" name="批准" to="end"/> 
      </decision> 
      <end-cancel g="384,441,48,48" name="cancel"/> 
    </process> 
 

MyEclipse工程

![](http://i1298.photobucket.com/albums/ag53/lichengwu/2_zps17e8da41.gif)

[工程下载](http://cid-2c8a0dc7c1eb1d71.skydrive.live.com/self.aspx/soft/leave%5E_web.rar)


{% include JB/setup %}