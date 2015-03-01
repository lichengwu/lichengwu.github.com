---
layout: post
title: "Swift技巧，持续更新"
description: "汇总一下初学swift遇到的问题"
category: ios
subhead: '汇总一下初学swift遇到的问题'
tags: [swift, ios]

---

<br >

#### 1.NSURL含有中文，会导致创建NSURL的结果是nil，把字符串用UTF8转码一下就好了:

    var url = NSURL(string: "http://wthrcdn.etouch.cn/weather_mini?city=北京".stringByAddingPercentEscapesUsingEncoding(NSUTF8StringEncoding)!)


{% include JB/setup %}
