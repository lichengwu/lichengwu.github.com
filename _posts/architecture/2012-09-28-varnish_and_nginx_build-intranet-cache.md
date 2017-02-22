---
layout: post
catalog: true
title: "varnish+nginx实现内网附件缓存"
description: ""
category: architecture
subhead: ''
tags: [varnish,  nginx, architecture, cache]

---

本文是对文件[内网缓存方案](http://blog.lichengwu.cn/architecture/2012/04/19/internal-cache/)的实现。
 
 
varnish作为缓存服务，部署在内网192.168.0.220，varnish只能本机访问(nginx)，内网用户不能直接访问varnish，需要通过nginx代理来访问。

nginx作为varnish的代理，如果将来有更大规模的缓存，可以做负载均衡。

HttpSecureLinkModule 对请求(超时，防盗)验证，每个跳转到内网的url(带token和超时时间)都要经过nginx的验证，而且这个url会在很短时间失效，这样防止了内网用户盗用链接。
 
下面是一个请求的流程图：

![image](/images/architecture/cache_varnish_zps682d4519.png)

配置地址：https://gist.github.com/3797290
 
varnish配置：
 
 
    backend default {  
              .host = "hostname1.com";         
    }  
  
    acl access {  
            "192.168.0.220";  
            "localhost";  
    }  
 
    sub vcl_recv {  
            unset req.http.Cookie;  
            set req.grace = 3m;  
            #set req.url = regsub(req.url,"^(.+)(&token=.+)$","\1:");  
            if(req.url ~ "/varnish-ping"){  
                    error 200 "OK";  
            }  
            if(!client.ip ~ access){  
                    error 405 "Access Denied !";  
            }  
          
            if(req.url ~ ".*/attachment/.*"){  
                    set req.http.host = "file.hostname";  
                    set req.url = regsub(req.url,"/attachment/(.+)(&token=.+)$","/cache/attachment/\1");  
                    #return (lookup);  
            }  
            return (lookup);  
    }  
  
    sub vcl_fetch {  
            if (beresp.status == 500) {  
                    error 404 "File not Found!";  
            }  
            if (beresp.http.Content-Length == "0" || beresp.http.Content-Length == "" ) {  
                    return (hit_for_pass);  
            }  
  
            set beresp.ttl = 90d;  
            set beresp.grace = 5m;  
            return (deliver);  
    }           
 
nginx配置：
  
    user  root;  
    worker_processes  2;  
  
    #error_log  logs/error.log;  
    #error_log  logs/error.log  notice;  
    #error_log  logs/error.log  info;  
  
    #pid        logs/nginx.pid;  
  
  
    events {  
        worker_connections  1024;  
    }  
  
  
    http {  
        include       mime.types;  
        default_type  application/octet-stream;  
  
        #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '  
        #                  '$status $body_bytes_sent "$http_referer" '  
        #                  '"$http_user_agent" "$http_x_forwarded_for"';  
  
        #access_log  logs/access.log  main;  
  
        sendfile        on;  
        #tcp_nopush     on;  
  
        #keepalive_timeout  0;  
        keepalive_timeout  65;  
  
        #gzip  on;  
  
        server {  
            listen       80;  
            server_name  192.168.0.220;  
  
            #charset koi8-r;  
  
            #access_log  logs/host.access.log  main;  
  
                    #varnish_host for get cache  
                    set $varnish_host       http://192.168.0.220:9350;  
  
                    # ping for check varnish health  
                    location ~ varnish-ping$ {  
                            allow   all;  
                            proxy_pass      $varnish_host;  
                    }  
  
                    # for contract  
                    location / {  
                            allow   all;  
                            root    html;  
                            proxy_pass $varnish_host;  
                            secure_link $arg_token,$arg_expire;  
                            secure_link_md5 my_keys_string$uri$arg_expire;  
  
                            if ($secure_link = "") {  
                                    return 403;  
                            }  
  
                            if ($secure_link = "0") {  
                                                                return 404;  
                            }  
                            #if ( $args ~ ^(.+)(&token=.+)$ ) {  
                            #       set $args $1;  
                            #       set $varnish_host   http://192.168.0.220:9350;  
                            #       rewrite ^(.+)$ $varnish_host$uri?$1 break;  
                            #       #rewrite ^(.*)$ "http://www.meituan.com"$args break;  
                            #}  
                    }  
  
            #error_page  404              /404.html;  
  
            # redirect server error pages to the static page /50x.html  
            #  
            error_page   500 502 503 504  /50x.html;  
            location = /50x.html {  
            root   html;  
            }  
        }  
    }  
 
 



