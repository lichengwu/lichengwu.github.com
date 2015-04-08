---
layout: post
title: "nginx query string rewrite"
description: ""
category: linux
subhead: ''
tags: [nginx,  urlrewrite]

---

有url

    http://192.168.0.1:9988/app/file_view?id=df0de9d8-8a0b-11e1-9ddb-0026b93f2307&token=TASIpYr6mj2-h78JEQ5ymg&expire=1334834398
    
想rewrite成

    http://192.168.0.1:8899/app/file_view?id=df0de9d8-8a0b-11e1-9ddb-0026b93f2307
    
也就是去掉token和expire参数

方法：

rewrite不涉及query string，只是uri，所以：

    proxy_pass http://192.168.0.1:8899;
    rewrite ^(.+)&token=.* $1 break; 

结果还是有token和expire参数。

采用if，改变$args变量，重新拼接url

    set $varnish_host http://192.168.0.1:8899;
    if ( $args ~ ^(.+)(&token=.+)$ ) {
        set $args $1;
        rewrite ^ $varnish_host$uri break; 
    }
    	
参考：[http://wiki.nginx.org/HttpCoreModule#.24args](http://wiki.nginx.org/HttpCoreModule#.24args)


