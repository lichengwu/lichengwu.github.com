---
layout: post
title: "ubuntu LAMP 解决验证码不能显示问题 安装gd2库"
description: ""
category: linux
subhead: ''
tags: [ubuntu, linux, LAMP, gd2, captcha]

---

刚安装的LAMP 无法显示验证码，原来是gd库没有安装。

#### 打开终端：
 
    root@oliver-laptop:~# sudo apt-get install php5-gd
    正在读取软件包列表... 完成
    正在分析软件包的依赖关系树
    正在读取状态信息... 完成
    将会安装下列额外的软件包：
    libt1-5
    下列【新】软件包将被安装：
    libt1-5 php5-gd
    共升级了 0 个软件包，新安装了 2 个软件包，要卸载 0 个软件包，有 0 个软件未被升级。
    需要下载 187kB 的软件包。
    解压缩后会消耗掉 557kB 的额外空间。
    您希望继续执行吗？[Y/n]y
    获取：1 http://mirrors.163.com karmic/main libt1-5 5.1.2-3 [154kB]
    获取：2 http://mirrors.163.com karmic-security/main php5-gd 5.2.10.dfsg.1-2ubuntu6.4 [33.1kB]
    下载 187kB，耗时 1 秒 (126kB/s)
    选中了曾被取消选择的软件包 libt1-5。
    (正在读取数据库 ... 系统当前总共安装有 147145 个文件和目录。)
    正在解压缩 libt1-5 (从 .../libt1-5_5.1.2-3_i386.deb) ...
    选中了曾被取消选择的软件包 php5-gd。
    正在解压缩 php5-gd (从 .../php5-gd_5.2.10.dfsg.1-2ubuntu6.4_i386.deb) ...
    正在设置 libt1-5 (5.1.2-3) ...
    
    正在设置 php5-gd (5.2.10.dfsg.1-2ubuntu6.4) ...
    
    正在处理用于 libc-bin 的触发器...
    ldconfig deferred processing now taking place
 
 
#### 重启apache2:

    root@oliver-laptop:~# service apache2 restart
    * Restarting web server apache2
    apache2: Could not reliably determine the server's fully qualified domain name, using 127.0.1.1 for ServerName
    ... waiting apache2: Could not reliably determine the server's fully qualified domain name, using 127.0.1.1 for ServerName
    [ OK ]

这样就OK了！

