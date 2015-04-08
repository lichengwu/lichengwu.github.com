---
layout: post
title: "VS2008 安装失败解决方案"
description: ""
category: .net
subhead: ''
tags: [.net, vs2008]

---

之前在2003系统中安装了Visual Studio Team System 2008 Team Suite，很顺利就能完成。但是这次在XP SP2安装Professional版本时出现了如下错误：

![image](/images/c-sharp/1_zps5bc5f631.png)

![image](/images/c-sharp/2_zps73405d6c.png)
 
在安装VS Web创作组件时发生了异常，致使其它的组件也不能安装。很是纳闷，求助网络，得到解决方案。

#### 解决方案： 
在安装DVD的/WCU/WebDesignerCore文件夹中找到了“Visual Studio Web 创作组件”的安装程序，将其复制到硬盘上并解压缩，例如加压至A文件夹。然后找到OFFICE2007安装包，解压，找到里面的office.zh-cn，直接将整个文件夹COPY至A文件夹中，然后点击SETUP,则，此组件就能正常安装了。具体原因不明，可能是微软里面的OFFICE安装包不是最新版本。

原文地址：http://www.cnblogs.com/yank/archive/2009/03/07/1405487.html

