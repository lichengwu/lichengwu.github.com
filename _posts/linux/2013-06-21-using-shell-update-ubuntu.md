---
layout: post
catalog: true
title: "纯命令行方式升级ubuntu系统"
description: ""
category: linux
subhead: ''
tags: [ubuntu, linux, update]

---

1)先升级系统软件：

    sudo apt-get update&&sudo apt-get upgrade

2)重启电脑后安装update-manager-core：

    sudo apt-get install update-manager-core

3)然后编辑`/etc/update-manager/release-upgrades`

将`Prompt=lts`改成`Prompt=normal`
然后保存关闭

4)最后执行升级：

    sudo do-release-upgrade -d
    
6)按照提示进行操作就可以了