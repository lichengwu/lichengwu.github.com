---
layout: post
title: "中文ubuntu里用户目录里的路径改成英文"
description: ""
category: linux
subhead: ''
tags: [ubuntu, linux]

---

为了使用起来方便，装了ubuntu中文版，自然在home文件里用户目录的“桌面”、“图片”、“视频”、“音乐”……都是中文的。

很多时候都喜欢在桌面上放一些要操作的文件，linux里命令行操作又多，难免会用命令行操作桌面上的东西，那么就要 “cd  桌面”，打“桌面”的时候要输入法切换，麻烦……所以就想办法把用户目录下的路径改成英文，而其他的中文不变，方法如下：

打开终端，在终端中输入命令:  

        export LANG=en_US
        xdg-user-dirs-gtk-update
        
跳出对话框询问是否将目录转化为英文路径,同意并关闭.

在终端中输入命令:

        export LANG=zh_CN

关闭终端,并重起.下次进入系统,系统会提示是否把转化好的目录改回中文.选择不再提示,并取消修改.主目录的中文转英文就完成了~

原文地址：http://my.oschina.net/myriads/blog/2867


