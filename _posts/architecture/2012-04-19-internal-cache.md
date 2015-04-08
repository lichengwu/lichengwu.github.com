---
layout: post
title: "文件内网缓存方案"
description: ""
category: architecture
subhead: ''
tags: [cache,  varnish,  ngnix, architecture]

---

#### 需求:

有一些图片，doc文件，公司内网用户需要经常访问，每次每个人都从服务器上下载，非常耗时。

一些文件可以根据规则打成zip包下载。

需要在内网架设缓存服务，加快公司内网访问速度。

文件资源敏感，不能随便访问，每次访问都要有权限验证和日志记录(即使访问内网)。

#### 架构：
 
采用varnish做缓存，varnish对用户透明。

采用nginx+Secure Link做保护内网url安全访问（验证，超时）。

具体流程：

![image](/images/architecture/cache_zpsae90b411.png)


#### 说明：
 
对于单个文件(图片,doc)，永久缓存。

对于打包下载文件，采用版本管理。版本=打包文件中添加时间最大的时间戳。如果版本过期，重新从线上打包并缓存。

