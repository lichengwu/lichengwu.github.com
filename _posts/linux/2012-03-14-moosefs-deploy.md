---
layout: post
catalog: true
title: "MooseFS 分布式文件系统介绍与部署"
description: ""
category: linux
subhead: ''
tags: [moosefs, dfs]

---

#### 一些概念

**Master** 用来管理MooseFS。安装master的主机需要稳定，有一定的可用内存，一台服务器即可。

**Metalogger master** 一些元数据备份。必要时可以恢复数据，至少一台服务器。

**Chunkservers** 文件块的存储服务，推荐至少有两台服务。

**Clients** 通过mount访问Chunkservers文件。

详情参考：[http://www.moosefs.org/reference-guide.html](http://www.moosefs.org/reference-guide.html)

#### MooseFS的优点

 * 通过挂载映射，能像访问本地文件的方式访问文件服务器资源
 
 * 支持特殊文件，如：block and character devices, pipes and sockets

 * 支持热部署，扩展不用停服务

 * 删除文件可以预留一段时间才真正删除

#### MooseFS工作原理

##### 1.process read

![image](/images/linux/1_zps15da9fd7.png)

1. 询问master server要访问的文件在哪个文件服务器

2. master server回应文件位置

3. 客户端像chunk server请求数据

4. chunk server返回数据

##### 2.process write

![image](/images/linux/2_zps3344c68f.png)

1. 客户端向master server询问文件存储位置

2. master server在每个chunk server上创建数据块用来存储文件

3. master server通知客户端往某一台chunk server上写数据

4. 客户端写数据到某一个chunk server

5. chunk server之间同步数据

6. chunk server之间数据同步成功

7. chunk server数据同步成功，返回给客户端

8. 客户端发终止写信号给master server

#### 线下配置

Master server: dog

Metalogger server: dog

Chunk servers: dog,dev,自己的虚拟机

Users'computer(客户端):dog,自己的虚拟机

#### 安装

线下安装版本mfs-1.6.20-2
安装向导：[http://www.moosefs.org/tl_files/manpageszip/moosefs-step-by-step-tutorial-cn-v.1.1.pdf](http://www.moosefs.org/tl_files/manpageszip/moosefs-step-by-step-tutorial-cn-v.1.1.pdf)

<span style="color: red">ubuntu安装fuse可以成功，如果是centos不能安装成功，yum install fuse-devel即可解决问题。</span>

#### 部署目录

1. 启动服务：
master server 进程: /usr/sbin/mfsmaster start

metalogger 进程: /usr/sbin/mfsmetalogger start

chunk server 进程: /usr/sbin/mfschunkserver start

启动监控：/usr/sbin/mfscgiser

2. 启动客户端
   
    #mkdir /mnt/javafiles  
    #chown -R sankuai:sankuai /mnt/javafiles  
    #mfsmount /mnt/javafiles -H mfsmaster  
    #mv -f uploadFiles uploadFiles2  
    #ln -s /mnt/javafiles uploadFiles  
    #chown -R sankuai:sankuai uploadFiles  
    #cp -rf /uploadFiles2/* /mnt/javafiles     
    #rm -rf /uploadFiles2  
 
 
监视地址：http://dog:9425

设置副本：因为只有2台chunk server 所以设置副本为2
  
     #mfssetgoal -r 2 /mnt/javafiles/ 
      
#### 还需要的工作

机器重启保证服务自动运行，和重启后mfsmount自动挂载。

解决单点问题： [http://hi.baidu.com/leolance/blog/item/7ac035205870f020c9955905.html](http://hi.baidu.com/leolance/blog/item/7ac035205870f020c9955905.html)


