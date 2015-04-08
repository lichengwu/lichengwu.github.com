---
layout: post
title: "Tomcat6下使用jBPM-4出现 java.lang.LinkageError。javax/el/ExpressionFactory解决办法"
description: ""
category: java
subhead: ''
tags: [jbpm,  java,  tomcat]

---

这个错误恶心了我一天，现在终于解决了。

### 以下是找到的方法：
因为tomcat6下的el-api.jar与jBPM-4使用的juel.jar产生冲突。

##### 解决方法一：
改用tomcat-5.5。

##### 解决方法二：
将juel.jar, juel-engine.jar, juel-impl.jar三个文件复制到tomcat的lib目录下，删除原有的el-api.jar即可解决。
 
 
 
原文：[http://yaowei06252009.javaeye.com/blog/608334](http://yaowei06252009.javaeye.com/blog/608334)
 

