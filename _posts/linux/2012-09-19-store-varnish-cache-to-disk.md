---
layout: post
title: "varnish 缓存到硬盘"
description: ""
category: linux
subhead: ''
tags: [varnish, linux]

---

#### 方法一：在启动的时候设置
   
    varnishd -s file,/var/lib/varnish/varnish_storage.bin,50%  
 
#### 方法二：修改默认启动文件
 
   
    vi /etc/default/varnish  
  
    AEMON_OPTS="-a :9350 \  
                -T localhost:9351 \  
                -f /etc/varnish/default.vcl \  
                -S /etc/varnish/secret \  
                -s file,/mnt/varnish/varnish_storage.bin,200G"  
 
参考：[http://nwlinux.com/varnish-file-storage-malloc-or-file/](http://nwlinux.com/varnish-file-storage-malloc-or-file/)