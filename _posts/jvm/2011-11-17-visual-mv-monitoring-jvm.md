---
layout: post
title: "利用VisualVM监视远程JVM"
description: ""
category: jvm
subhead: ''
tags: [java, jvm, VisualVM, monitoring]

---

#### VisualVM介绍
VisualVM是集成了多个JDK命令工具的一个可视化工具，它主要用来监控JVM的运行情况，可以用它来查看和浏览Heap Dump、Thread Dump、内存对象实例情况、GC执行情况、CPU消耗以及类的装载情况。

在JDK Update7之后，VisualVM作为JDK的一部分发布，但同时VisualVM也发布独立的版本。VisualVM必须运行在JDK1.6以上的VM环境下，但可以用它来监控JDK1.4以上的JVM

下载地址：[http://visualvm.java.net/download.html](http://visualvm.java.net/download.html)

#### 配置jetty------匿名
修改启动脚本：

    vi /srv/jetty6/mtct

在RUN_CMD后面追加：
指定hostname 一般情况需要重新指定hostname,否则连接不成功

    -Djava.rmi.server.hostname=192.168.0.147
指定hostname 指定端口默认：1099

    -Dcom.sun.management.jmxremote.port=8899
禁止ssl连接

    -Dcom.sun.management.jmxremote.ssl=false

禁止用户认证

    -Dcom.sun.management.jmxremote.authenticate=false

#### 另一种配置------认证配置

指定hostname 一般情况需要重新指定hostname,否则连接不成功

    -Djava.rmi.server.hostname=192.168.0.147
指定hostname 指定端口默认：1099

    -Dcom.sun.management.jmxremote.port=8899

禁止ssl连接

    com.sun.management.jmxremote.ssl=false

开启用户认证

    com.sun.management.jmxremote.authenticate=true

认证用户名密码

    -Dcom.sun.management.jmxremote.password.file=/opt/home/lichengwu/jvm/management/jmxremote.password

访问模式

    -Dcom.sun.management.jmxremote.access.file=/opt/home/lichengwu/jvm/management/jmxremote.access

注意：jmxremote.password和jmxremote.access文件只允许启动用户名对该文件拥有读写权限 ，我们服务用root启动 

所以：

    [root@dog:management]# chmod 600 *
    [root@dog:management]# chown root:root *
    [root@dog:management]# ll
    total 8
    -rw------- 1 root root 29 Nov 14 16:38 jmxremote.access
    -rw------- 1 root root 26 Nov 14 16:38 jmxremote.password
    [root@dog:management]#

如果权限设置不正确会报错：Error: Password file read access must be restricted
jmxremote.password

模板：

    [用户名]       [密码]
    mtct          ct.meituan
    test          test
jmxremote.access

模板：

    [用户名]      [权限]
    mtct        readwrite
    test        readonly
#### 第三种配置------SSL

参考：[http://download.oracle.com/javase/1.5.0/docs/guide/management/agent.html#SSL_enabled](http://download.oracle.com/javase/1.5.0/docs/guide/management/agent.html#SSL_enabled)
在服务器上使用keytool创建密钥对

keytool是java平台自带的一个密钥和证书管理工具，使用keytool创建密钥对：

keytool -genkey -alias jetty -keystore /opt/home/lichengwu/jvm/ssl/jettyKeyStore

按照提示输入相关信息（包括设定密码、姓、组织名等），这些信息是可以随便输入的，但从产品角度讲应该统一设定。输入的
密码在今后操作中均需要使用。

导出公钥

    keytool -export -alias jetty -keystore /opt/home/lichengwu/jvm/ssl/jettyKeyStore -file /opt/home/lichengwu/jvm/ssl/jetty.cert

将公钥导入至需要运行VisualVM的机器。(我的是windows 放在 Z:\jvm\ssl\jetty.cert)

    keytool -import
     -alias jetty -keystore Z:\jvm\ssl\jettyKeyStore -file Z:\jvm\ssl\jetty.cert
修改jetty的启动脚本

将-Dcom.sun.management.jmxremote.ssl=`"false"`修改为：-Dcom.sun.management.jmxremote.ssl=`"true"`，并添加：

    -Djavax.net.ssl.keyStore=/opt/home/lichengwu/jvm/ssl/jettyKeyStore
    -Djavax.net.ssl.keyStorePassword=123456
    
使用如下参数启动VisualVM：

    VisualVM -J-Djavax.net.ssl.trustStore=Z:\jvm\ssl\jettyKeyStore

####监控

启动VisualVM，添加远程主机：

![image](http://i1298.photobucket.com/albums/ag53/lichengwu/1_zpseedc55cc.png)

输入远程主机地址：192.168.0.147

![image](http://i1298.photobucket.com/albums/ag53/lichengwu/2_zps30d064e6.png)

修改端口，如果是默认端口，可可跳过

![image](http://i1298.photobucket.com/albums/ag53/lichengwu/3_zps315c11ae.png)

添加JMX连接

![image](http://i1298.photobucket.com/albums/ag53/lichengwu/4_zps8b2d502d.png)

完成后双击：

![image](http://i1298.photobucket.com/albums/ag53/lichengwu/5_zpsb1050a31.png)


{% include JB/setup %}