---
layout: post
title: "daemontools安装使用"
description: ""
category: linux
subhead: ''
tags: [daemontools, linux]

---

#### Create a /package directory:
    mkdir -p /package
    chmod 1755 /package
    cd /package
    
#### Download daemontools-0.76.tar.gz into /package.Unpack the daemontools package:
    gunzip daemontools-0.76.tar
    tar -xpf daemontools-0.76.tar
    rm -f daemontools-0.76.tar
    cd admin/daemontools-0.76
    sed -i -e 's/-Wwrite-strings/-Wwrite-strings -include \/usr\/include\/errno.h/g' src/conf-cc 
    #添加辅助脚本并解决编译错误
 
 
    /usr/bin/ld: errno: TLS definition in /lib/libc.so.6 section .tbss mismatches non-TLS reference in envdir.o  
    /lib/libc.so.6: could not read symbols: Bad value  
    collect2: ld returned 1 exit status  
 
#### Compile and set up the daemontools programs:

    package/install
    
#### Install to inittab：
    
    package/run.inittab
    
#### Usage：

    mkdir /service/your_service_name
    cd /service/your_service_name
    vi run #启动程序的脚步
    chmod 755 run
 
#### Check status
    svstat /service/srv1
#### Stop
    svc -d /service/srv1
#### Start
    svc -u /service/srv1

{% include JB/setup %}