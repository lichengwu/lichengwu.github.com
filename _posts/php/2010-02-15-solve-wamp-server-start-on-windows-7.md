---
layout: post
title: "关于wamp server在Windows7 x86&x64上无法正常启动运行的解决方法。"
description: ""
category: php
subhead: ''
tags: [apache,  httpd,  wamp,  php]

---

本人系统Windows7 x64 安装wamp后无法启动apache服务器。

开始以为是apache与系统的兼容问题，后来才知道是端口问题。

启动wamp后图标是黄色的。

解决方法，打开apache的配置文件httpd.conf文件，找到这行：
`Listen 80`

修改为 `Listen 8088`，保存退出。

重启apache，访问http://localhost:8088/ 就可以了。
 
如果修改apache的配置文件httpd.conf文件，找到
`ServerName localhost:80`
修改为：`ServerName localhost:8088`.

重启apache，访问http://localhost 也可以。


