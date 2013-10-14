---
layout: post
title: "常用linux命令(不断更新)"
description: "常用linux命令"
category: linux
subhead: ''
tags: [linux, shell]

---
 
#### 错误和正确都重定向
    sh... > test.log 2>&1
#### 首字母大写替换
    sed 's/\b[a-z]/\U&/g
#### 删除0字节文件
    find -type f -size 0 -exec rm -rf {} \;
#### 查看进程(按内存从大到小排列)
    ps -e -o "%C : %p : %z : %a"|sort -k5 -nr
#### 按cpu利用率从大到小排列
    ps -e -o "%C : %p : %z : %a"|sort -nr
#### 查看http的并发请求数及其TCP连接状态：
    netstat -n | awk '/^tcp/ {++S[$NF]} END {for(a in S) print a, S[a]}'
#### 磁盘I/O负载(检查I/O使用率(%util)是否超过100%)
    iostat -x 1 2
#### 网络负载
    sar -n DEV
#### 网络错误
    netstat -i
#### 网络连接数目
    netstat -an | grep -E “^(tcp)” | cut -c 68- | sort | uniq -c | sort -n
#### 打开文件数目
    lsof | wc -l
#### 杀掉80端口相关的进程
    lsof -i :80|grep -v "PID"|awk '{print "kill -9",$2}'|sh
#### 清除僵死进程
    ps -eal | awk '{ if ($2 == "Z") {print $4}}' | kill -9
#### 统计代码行数(去空格)
    find . -name "*.java"|xargs cat|grep -v ^$|wc -l
#### 查找并删除文件/目录
    find . -name "" | xargs rm -rm
#### 跳板机免登录 将如下设置添加到个人的ssh配置文件中(.ssh/config)，即可共享第一次登录的连接：

    Host *
    ControlPath ~/.ssh/master-%r@%h:%p
    ControlMaster auto

#### 删除自动启动的服务
    sudo update-rc.d -f mysql remove 
   
#### TOP线程模式
    top -H

#### 查看线程启动时间
    ps -p {PID} -o lstart

{% include JB/setup %}
