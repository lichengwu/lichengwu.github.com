---
layout: post
title: "http_load工具使用"
description: ""
category: linux
subhead: ''
tags: [tool,  http_load]

---

#### 安装：
 
    #wget http://soft.vpser.net/test/http_load/http_load-12mar2006.tar.gz  
    #tar zxvf http_load-12mar2006.tar.gz  
    #cd http_load-12mar2006  
    #make && make install 
     
#### 用法：

    usage:  http_load [-checksum] [-throttle] [-proxy host:port] [-verbose] [-timeout secs] [-sip sip_file]
            -parallel N | -rate N [-jitter]
            -fetches N | -seconds N
            url_file
    One start specifier, either -parallel or -rate, is required.
    One end specifier, either -fetches or -seconds, is required.
    
#### 参数说明：
-parallel 简写-p ：含义是并发的用户进程数

-fetches 简写-f ：含义是总计的访问次数

-rate    简写-r ：含义是每秒的访问频率

-seconds简写-s ：含义是总计的访问时间
 

{% include JB/setup %}