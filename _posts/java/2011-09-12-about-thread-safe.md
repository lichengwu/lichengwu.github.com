---
layout: post
title: "并发环境下，我们的工具类是否安全？"
description: ""
category: java
subhead: ''
tags: [java, concurrency]

---


由于web天生并发性，导致我们的一般java工具类会在这样的环境下出现问题。

其实问题的根源就是我们的工具类不是线程安全的。

有一个生成md5的工具类：
  
    public class MD5 {  
        private static long[] state = new long[4];  
        private static long[] count = new long[2];  
        private static byte[] buffer = new byte[64];  
        private static byte[] digest = new byte[16];  
      
        private String digestHexStr="";  
        public static MD5() {  
        }  
        //计算MD5  
        public static String getMD5ofStr(String inbuf) {  
        }  
    }  
 
变量state， count ，buffer ，digest 算法中用到的核心数据，digestHexStr存放计算的结果。在多线程并发访问的情况下，这些变量是会被“共享”的，所以会导致计算结果不准确甚至出现异常。

有三种比较简单的方法可以解决：

getMD5ofStr方法变成非static的普通方法，这样每次调用这个方法都必须new一个新的MD5对象。

getMD5ofStr方法变成同步方法（同步代码块，显示锁，synchronized method都可以）。

将被“共享”的变量放到方法getMD5ofStr里面，不设置成员变量。

考虑到现在系统有些地方已经开始使用这个工具类了，不便改动结构，先采用第二种快速修复bug，然后腾出时间用第三种发放重构。

PS：

工具类能否设计成单例？如果能最好。单例能减少创建类和分配内存的开销，减少垃圾回收次数。

工具类能否设计成不变类？如果能最好，不变类天生线程安全！

在并发环境下，工具类能不能不用同步？不管怎么说，同步都是要有一些开销的。

PPS：

这样会好一些：

    public final class MD5 {  
        private MD5(){}  
        //计算MD5  
        public static String getMD5ofStr(String inbuf) {  
            long[] state = new long[4];  
            long[] count = new long[2];  
            byte[] buffer = new byte[64];  
            byte[] digest = new byte[16];  
            String digestHexStr="";  
            ........  
        }  
    }  
 
 


