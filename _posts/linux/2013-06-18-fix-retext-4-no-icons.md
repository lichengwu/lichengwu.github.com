---
layout: post
title: "ubuntu解决ReText４无法显示按钮图标"
description: ""
category: linux
subhead: ''
tags: [ubuntu, linux, ReText]

---

ReText是一个用python写的markdown编辑器，底层使用了Qt类库。无法显示图标的原因是Qt的一个bug。但是这个问题很容易解决。

####ReText安装

    sudo add-apt-repository ppa:mitya57
    sudo apt-get update
    sudo apt-get install retext
    
安装发现按钮的图标都无法显示：

![](http://i1298.photobucket.com/albums/ag53/lichengwu/NewdocumentmdashReText_001_zpse77c6b89.png)

#### 解决方法:

    $ mkdir -p "~/.config/ReText project/"
    $ echo "iconTheme=Humanity" >> "~/.config/ReText project/ReText.conf"
    
#### 参考:

[https://bugs.launchpad.net/ubuntu/+source/qt4-x11/+bug/1180067](https://bugs.launchpad.net/ubuntu/+source/qt4-x11/+bug/1180067)

[http://www.svenbit.com/2012/05/fix-missing-toolbar-icon-in-retext/](http://www.svenbit.com/2012/05/fix-missing-toolbar-icon-in-retext/)
    