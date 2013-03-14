---
layout: post
title: "jinfo 查看、设置JVM参数"
description: ""
category: jvm
subhead: ''
tags: [jinfo,  jvm]

---

#### 用法：

    # jinfo -h  
    Usage:  
        jinfo [option] <pid>  
            (to connect to running process)  
        jinfo [option] <executable <core>  
            (to connect to a core file)  
        jinfo [option] [server_id@]<remote server IP or hostname>  
            (to connect to remote debug server)  
  
    where <option> is one of:  
        -flag <name>         to print the value of the named VM flag  
        -flag [+|-]<name>    to enable or disable the named VM flag  
        -flag <name>=<value> to set the named VM flag to the given value  
        -flags               to print VM flags  
        -sysprops            to print Java system properties  
        <no option>          to print both of the above  
        -h | -help           to print this help message  
#### 例子：
 
    $ jinfo -flag HeapDumpBeforeFullGC  29167  #查看HeapDumpBeforeFullGC  
    -XX:-HeapDumpBeforeFullGC  
    $ jinfo -flag +HeapDumpBeforeFullGC  29167  #打开HeapDumpBeforeFullGC  
    $ jinfo -flag HeapDumpBeforeFullGC  29167  
    -XX:+HeapDumpBeforeFullGC  
    $ jinfo -flag -HeapDumpBeforeFullGC  29167  #关闭HeapDumpBeforeFullGC  
    $ jinfo -flag HeapDumpBeforeFullGC  29167  
    -XX:-HeapDumpBeforeFullGC  

如果出现：
 
    pid: well-known file is not secure 
     
请确认当前执行jinfo的命令的用户有/tmp/hsperfdata_$USER/$PID 文件的权限

如果出现：

    Exception in thread "main" java.io.IOException: Command failed in target VM  
            at sun.tools.attach.LinuxVirtualMachine.execute(LinuxVirtualMachine.java:218)  
            at sun.tools.attach.HotSpotVirtualMachine.executeCommand(HotSpotVirtualMachine.java:213)  
            at sun.tools.attach.HotSpotVirtualMachine.setFlag(HotSpotVirtualMachine.java:190)  
            at sun.tools.jinfo.JInfo.flag(JInfo.java:123)  
            at sun.tools.jinfo.JInfo.main(JInfo.java:76)  
            
 说明参数不支持修改
 
 另外，`jinfo可能在未来的jvm中移除`。
 
#### 参考资料：
 
[http://bugs.sun.com/bugdatabase/view_bug.do?bug_id=4109888](http://bugs.sun.com/bugdatabase/view_bug.do?bug_id=4109888)

[http://blog.tsunanet.net/2010/10/ioexception-well-known-file-is-not.html](http://blog.tsunanet.net/2010/10/ioexception-well-known-file-is-not.html)

[http://docs.oracle.com/javase/6/docs/technotes/tools/share/jinfo.html](http://docs.oracle.com/javase/6/docs/technotes/tools/share/jinfo.html)

{% include JB/setup %}