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

<table class="table table-bordered table-striped table-condensed"><tbody>
<tr>
<th> Command Line Parameter </th>
<th> Description </th>
</tr>
<tr>
<td> --httpPort=$HTTP_PORT </td>
<td> Runs Jenkins listener on port $HTTP_PORT using standard <em>http</em> protocol. The default is port 8080. To disable (because you're using <em>https</em>), use port <tt>-1</tt>. </td>
</tr>
<tr>
<td> --httpListenAddress=$HTTP_HOST </td>
<td> Binds Jenkins to the IP address represented by $HTTP_HOST. The default is 0.0.0.0 — i.e. listening on all available interfaces. <br class="atl-forced-newline">
For example, to only listen for requests from localhost, you could use: --httpListenAddress=127.0.0.1 </td>
</tr>
<tr>
<td> --httpsPort=$HTTP_PORT </td>
<td> Uses HTTPS protocol on port $HTTP_PORT </td>
</tr>
<tr>
<td> --httpsListenAddress=$HTTPS_HOST </td>
<td> Binds Jenkins to listen for HTTPS requests on the IP address represented by $HTTPS_HOST. </td>
</tr>
<tr>
<td> --prefix=$PREFIX<br class="atl-forced-newline"> </td>
<td> Runs Jenkins to include the $PREFIX at the end of the URL.<br class="atl-forced-newline">
 For example, to make Jenkins accessible at <tt><b>http</b></tt><tt><b>://</b></tt><font color="green"><tt><b>myServer</b></tt></font><tt><b>:8080/jenkins</b></tt>, set --prefix=/jenkins </td>
</tr>
<tr>
<td> --ajp13Port=$AJP_PORT </td>
<td> Runs Jenkins listener on port $AJP_PORT using standard <em>AJP13</em> protocol. The default is port 8009. To disable (because you're using <em>https</em>), use port <tt>-1</tt>. </td>
</tr>
<tr>
<td> --ajp13ListenAddress=$AJP_HOST </td>
<td> Binds Jenkins to the IP address represented by $AJP_HOST. The default is 0.0.0.0 — i.e. listening on all available interfaces. </td>
</tr>
<tr>
<td> --argumentsRealm.passwd.$ADMIN_USER </td>
<td> Sets the password for user $ADMIN_USER. If Jenkins security is turned on, you must log in as the $ADMIN_USER in order to configure Jenkins or a Jenkins project. NOTE: You must also specify that this user has an <em>admin</em> role. (See next argument below). </td>
</tr>
<tr>
<td> --argumentsRealm.roles.$ADMIN_USER=admin </td>
<td> Sets that $ADMIN_USER is an administrative user and can configure Jenkins if Jenkins' security is turned on. See <a href="/display/JENKINS/Securing+Jenkins" title="Securing Jenkins">Securing Jenkins</a> for more information. </td>
</tr>
<tr>
<td> -Xdebug -Xrunjdwp:transport=dt_socket,address=$DEBUG_PORT,server=y,suspend=n </td>
<td> Sets debugging on and you can access debug on $DEBUG_PORT. </td>
</tr>
<tr>
<td> -<del>logfile=$LOG_PATH/winstone_`date +"%Y</del>%m-%d_%H-%M"`.log </td>
<td> Logging to desired file </td>
</tr>
<tr>
<td> -XX:PermSize=512M -XX:MaxPermSize=2048M -Xmn128M -Xms1024M -Xmx2048M </td>
<td> referring <a href="http://docs.oracle.com/javase/7/docs/technotes/tools/windows/java.html" class="external-link" rel="nofollow">to these options for Oracle Java</a> <br class="atl-forced-newline"> </td>
</tr>
</tbody></table>
  
编写一个启动脚本：
<script src="https://gist.github.com/lichengwu/7959859.js"></script>  
   

#### 遇到的一些问题

##### 中文乱码
在启动参数加入：

    JAVA_TOOL_OPTIONS=-Dfile.encoding=GBK 
    
##### ANSI color    
安装AnsiColor插件：[AnsiColor](https://wiki.jenkins-ci.org/display/JENKINS/AnsiColor+Plugin)

#####执行远程build脚本
jenkins要以admin执行远程机器的build的脚本，而admin账户禁止登录，但是有一个帐号有admin的权限，所以采用ssh方式远程执行命令。
提供一个python脚本，解决：
 
 - 自动输入密码
 - 模拟终端(pseudo-tty)
 - 用`nohup`,解决ssh断开，程序退出
 
 <script src="https://gist.github.com/lichengwu/7960277.js"></script>


{% include JB/setup %}
