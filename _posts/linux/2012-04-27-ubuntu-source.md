---
layout: post
title: "ubuntu源推荐"
description: ""
category: linux
subhead: ''
tags: [ubuntu, source]

---

国内的源东西不是很全，推荐下面的源。

    #####################FFICIAL UBUNTU REPOS ###################
    #############################################################

    ###### Ubuntu Main Repos
    deb http://cn.archive.ubuntu.com/ubuntu/ precise main restricted universe
    deb-src http://cn.archive.ubuntu.com/ubuntu/ precise main restricted universe

    ###### Ubuntu Update Repos
    deb http://cn.archive.ubuntu.com/ubuntu/ precise-security main restricted universe
    deb http://cn.archive.ubuntu.com/ubuntu/ precise-updates main restricted universe
    deb http://cn.archive.ubuntu.com/ubuntu/ precise-proposed main restricted universe
    deb http://cn.archive.ubuntu.com/ubuntu/ precise-backports main restricted universe
    deb-src http://cn.archive.ubuntu.com/ubuntu/ precise-security main restricted universe
    deb-src http://cn.archive.ubuntu.com/ubuntu/ precise-updates main restricted universe
    deb-src http://cn.archive.ubuntu.com/ubuntu/ precise-proposed main restricted universe
    deb-src http://cn.archive.ubuntu.com/ubuntu/ precise-backports main restricted universe
    
    ###### Ubuntu Partner Repo
    deb http://archive.canonical.com/ubuntu precise partner
    deb-src http://archive.canonical.com/ubuntu precise partner

    ###### Ubuntu Extras Repo
    deb http://extras.ubuntu.com/ubuntu precise main
    deb-src http://extras.ubuntu.com/ubuntu precise main
    
如果想自定义源，请上：http://repogen.simplylinux.ch/

可以根据地区等自定义。

{% include JB/setup %}