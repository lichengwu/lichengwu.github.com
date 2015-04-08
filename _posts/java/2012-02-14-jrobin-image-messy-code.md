---
layout: post
title: "解决jrobin图像中文乱码"
description: ""
category: java
subhead: ''
tags: [java, jrobin, messy-code, rrd]

---

目前发现一种方法可以解决，做个标记。

利用字体：
  
    RrdGraphDef graphDef = new RrdGraphDef();  
    graphDef.setSmallFont(new Font(Font.MONOSPACED, Font.PLAIN, 10));  
    graphDef.setLargeFont(new Font(Font.MONOSPACED, Font.BOLD, 12));  
    ....  

