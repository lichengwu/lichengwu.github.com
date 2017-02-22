---
layout: post
catalog: true
title: "ubuntu12.04上编译openjdk7"
description: ""
category: jvm
subhead: ''
tags: [jvm,  openjdk,  jdk,  hotspot]
---

#### 获取源码

##### 从openjdk代码仓库获取(比较慢)
安装mercurial

Mercurial是一个版本管理工具。

    sudo apt-get install mercurial
    
将以下内容添加到$HOME/.hgrc文件中，如果没有则自己创建一个：

    [extensions]
    forest=/home/lichengwu/hgforest-crew/forest.py
    fetch=
    
下载jdk7源码

    hg fclone http://hg.openjdk.java.net/jdk7/jdk7
    
##### 直接下载源码包(推荐) 

    # wget http://www.java.net/download/openjdk/jdk7/promoted/b147/openjdk-7-fcs-src-b147-27_jun_2011.zip
    # unzip openjdk-7-fcs-src-b147-27_jun_2011.zip
    
#### 安装编译必须组件

安装gcc、g++、make等

    sudo apt-get install build-essential
    
安装ant 1.7以上

    sudo apt-get install ant
    
安装XRender

    sudo apt-get install libxrender-dev   
    sudo apt-get install xorg-dev 

安装alsa

    sudo apt-get install libasound2-dev

Cups

    sudo apt-get install libcups2-dev

安装jdk6

一般ubuntu都自带，没有的话

     sudo apt-get install openjdk-6-jdk

或者下载oracle的jdk也可以

安装杂碎

    sudo apt-get install gawk zip libxtst-dev libxi-dev libxt-dev

安装findbugs??

我在ubuntu12.04上没有安装findbugs也成功编译了，如果编译失败，请自行安装

#### 准备环境和脚本

    !/bin/bash
    # cd jdk source code folder
    cd ~/openjdk7
    # export ALT_BOOTDIR
    export LANG=C ALT_BOOTDIR=/home/lichengwu/openjdk_build_files/jdk1.6.0_32
    # set build profile
    jdk/make/jdk_generic_profile.sh
    # disable JAVA_HOME
    export -n JAVA_HOME
    # export ALT_JDK_IMPORT_PATH
    export ALT_JDK_IMPORT_PATH=/home/lichengwu/openjdk_build_files/jdk1.6.0_32
    # start build
    make DEBUG_NAME=all_fastdebug BUILD_JAXWS=false BUILD_JAXP=false

#### QA

如果你通过上面步骤就编译成功了，那你真的人品爆发了，以下是我编译时候遇到的问题

先注释掉 `WARNINGS_ARE_ERRORS = -Werror`

---
    vim hotspot/make/linux/makefiles/gcc.make
    "*** This OS is not supported:" `uname -a`; exit 1;
这是由于内核版本太高了，两种方式解决:

##### 方法一：

    lichengwu@s4:~/bin$ uname -r
    #查看当前的内核版本：
    3.2.0-20-generic

修改文件/home/thebye85/jdk7/hotspot/make/linux/Makefile

    #在这行最后加上当前的内核版本3.2%，如下：
    lichengwu@s4:~/bin$ SUPPORTED_OS_VERSION = 2.4% 2.5% 2.6% 2.7% 3.2%
    
##### 方法二：
    vi hotspot/make/linux/Makefile

注释掉下面代码：

    check_os_version:
    #ifeq ($(DISABLE_HOTSPOT_OS_VERSION_CHECK)$(EMPTY_IF_NOT_SUPPORTED),)
    # $(QUIETLY) >&2 echo "*** This OS is not supported:" `uname -a`; exit 1;
    #endif
    
----
    
sound错误

    cd jdk/make/javax/sound/jsoundalsa
    vim Makefile
    #找到CPPFLAGS ，在其结尾，添加 -lasound
    make[5]: *** [/home/lichengwu/openjdk7/build/linux-amd64/lib/amd64/libjsoundalsa.so] Error 1
    
解决：

    ln -s build/linux-amd64/lib/amd64/libjsound.so 
    build/linux-amd64/lib/amd64/libjsoundalsa.so 
    
如果ln不行 就用cp

#### 编译结果

corba hotspot langtools 编译时间很短是因为编译在hotspot时候出错，重新编译，如果正常一次跑通，大概半个小时 我用的是虚拟机:

 
    ########################################################################
    #####   Leaving jdk for target(s) sanity all  images               #####
    ########################################################################
    #####   Build time 00:09:14 jdk for  target(s) sanity all  images  #####
    ########################################################################
 
    -- Build times ----------
    Target all_fastdebug_build
    Start 2012-06-13 13:53:38
    End   2012-06-13 14:03:13
    00:00:07 corba
    00:00:08 hotspot
    00:09:14 jdk
    00:00:06 langtools
    00:09:35 TOTAL
    
#### 查看版本

    lichengwu@s4:~/openjdk7/build/linux-amd64/j2sdk-image/bin$ java -version
    java version "1.6.0_24"
 
    OpenJDK Runtime
 
    Environment (IcedTea6 1.11.1) (6b24-1.11.1-4ubuntu3)
    OpenJDK 64-Bit Server VM (build 20.0-b12, mixed mode)
    lichengwu@s4:~/openjdk7/build/linux-amd64/j2sdk-image/bin$ ./java -version
    openjdk version "1.7.0-internal-all_fastdebug"
 
    OpenJDK Runtime
 
    Environment (build 1.7.0-internal-all_fastdebug-lichengwu_2012_06_13_11_47-b00)
    OpenJDK 64-Bit Server VM (build 21.0-b17, mixed mode)

#### 参考资料：

编译：

[http://hg.openjdk.java.net/jdk7/build/raw-file/tip/README-builds.html](http://hg.openjdk.java.net/jdk7/build/raw-file/tip/README-builds.html)

[http://thebye85.iteye.com/blog/1545311](http://thebye85.iteye.com/blog/1545311)

关于为啥要自己编译jdk：

[http://rednaxelafx.iteye.com/blog/1549577](http://rednaxelafx.iteye.com/blog/1549577)

