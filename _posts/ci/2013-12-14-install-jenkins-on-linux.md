---
layout: post
title: "Jenkins安装使用"
description: ""
category: ci
subhead: ''
tags: [ci, jenkins]

---
#### 获取最新Jenkins

    wget http://mirrors.jenkins-ci.org/war/latest/jenkins.war
 
#### 启动Jenkins


简单启动：

    java -jar jenkins.war
    
支持的启动参数：





| Parameter | Description |
|-----------|-------------|| --httpPort=$HTTP_PORT | Runs Jenkins listener on port $HTTP_PORT using standard http protocol. The default is port 8080. To disable (because you're using https), use port -1. || --httpListenAddress=$HTTP_HOST | Binds Jenkins to the IP address represented by $HTTP_HOST. The default is 0.0.0.0 — i.e. listening on all available interfaces. For example, to only listen for requests from localhost, you could use: --httpListenAddress=127.0.0.1 || --httpsPort=$HTTP_PORT | Uses HTTPS protocol on port $HTTP_PORT || --httpsListenAddress=$HTTPS_HOST | Binds Jenkins to listen for HTTPS requests on the IP address represented by $HTTPS_HOST. || --prefix=$PREFIX | Runs Jenkins to include the $PREFIX at the end of the URL.For example, to make Jenkins accessible at http://myServer:8080/jenkins, set --prefix=/jenkins || --ajp13Port=$AJP_PORT | Runs Jenkins listener on port $AJP_PORT using standard AJP13 protocol. The default is port 8009. To disable (because you're using https), use port -1. || --ajp13ListenAddress=$AJP_HOST | Binds Jenkins to the IP address represented by $AJP_HOST. The default is 0.0.0.0 — i.e. listening on all available interfaces. || --argumentsRealm.passwd.$ADMIN_USER | Sets the password for user $ADMIN_USER. If Jenkins security is turned on, you must log in as the $ADMIN_USER in order to configure Jenkins or a Jenkins project. NOTE: You must also specify that this user has an admin role. (See next argument below). || --argumentsRealm.roles.$ADMIN_USER=admin | Sets that $ADMIN_USER is an administrative user and can configure Jenkins if Jenkins' security is turned on. See Securing Jenkins for more information. || -Xdebug -Xrunjdwp:transport=dt_socket,<br>address=$DEBUG_PORT,server=y,suspend=n | Sets debugging on and you can access debug on $DEBUG_PORT. || -logfile=$LOG_PATH/winstone_date +"%Y</del>%m-%d_%H-%M".log | Logging to desired file || -XX:PermSize=512M -XX:MaxPermSize=2048M -Xmn128M -Xms1024M -Xmx2048M | referring to these options for Oracle Java |

  
编写一个启动脚本：

    #!/bin/sh
     
    DESC="Jenkins CI Server"
    NAME=jenkins
    PIDFILE=/var/run/$NAME.pid
    RUN_AS=admin
    COMMAND=JAVA_TOOL_OPTIONS=-Dfile.encoding=GBK /opt/taobao/java64/bin/java -jar -server -Xms2048m -Xmx2048m -XX:NewSize=320m -XX:MaxNewS
    ize=320m -XX:PermSize=128m -XX:MaxPermSize=256m /usr/lib/jenkins/jenkins.war  --httpPort=9000
    #COMMAND=/opt/taobao/java/bin/java -jar /home/jenkins/jenkins.war
     
    d_start() {
            start-stop-daemon --start --quiet --background --make-pidfile --pidfile $PIDFILE --chuid $RUN_AS --exec $COMMAND
    }
     
    d_stop() {
            start-stop-daemon --stop --quiet --pidfile $PIDFILE
            if [ -e $PIDFILE ]
                    then rm $PIDFILE
            fi
    }
     
    case $1 in
            start)
            echo -n "Starting $DESC: $NAME"
            d_start
            echo "."
            ;;
            stop)
            echo -n "Stopping $DESC: $NAME"
            d_stop
            echo "."
            ;;
            restart)
            echo -n "Restarting $DESC: $NAME"
            d_stop
            sleep 1
            d_start
            echo "."
            ;;
            *)
            echo "usage: $NAME {start|stop|restart}"
            exit 1
            ;;
    esac
     
    exit 0

#### 遇到的一些问题

##### 中文乱码

在启动参数加入：

    JAVA_TOOL_OPTIONS=-Dfile.encoding=GBK 
    
##### ANSI color    
安装AnsiColor插件：[AnsiColor](https://wiki.jenkins-ci.org/display/JENKINS/AnsiColor+Plugin)

##### 执行远程build脚本

jenkins要以admin执行远程机器的build的脚本，而admin账户禁止登录，但是有一个帐号有admin的权限，所以采用ssh方式远程执行命令。
提供一个python脚本，解决:模拟终端(pseudo-tty)；用`nohup`,解决ssh断开，程序退出；自动输入密码。

 <script src="https://gist.github.com/lichengwu/7960277.js" />

