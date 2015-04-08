---
layout: post
title: "使用findbugs为自己的代码review"
description: "findbugs"
category: java
subhead: ''
tags: [java, findbugs, code review]

---

### 介绍
Findbugs是一个代码静态分析工具，用来找出Java代码中的bug。通过分析字节码并与一组缺陷模式匹配，找出代码中的问题。

目前Findbugs支持除了java 8以外的所有版本编译的字节码文件分析。   

Findbugs不仅提供了可视化界面，还有Idea,Eclipse插件。

Findbugs有谷歌和SUM/(Oracle)支持，在JDK源码编译中，依赖了Findbugs对JDK源码的检查。也就是说，虽然可以自由编译JDK，但是如果修改后的代码太烂，不能通过Findbugs的检查，一样不能通过编译。

### 使用

项目地址：[http://findbugs.sourceforge.net/](http://findbugs.sourceforge.net/)

Findbugs支持命令行，ant，可视化界面和IDE插件方式运行，以IDEA的插件使用为例。

Eclipse插件安装地址：[http://findbugs.cs.umd.edu/eclipse/](http://findbugs.cs.umd.edu/eclipse/)

IDEA插件安装地址：[http://plugins.jetbrains.com/plugin/3847](http://plugins.jetbrains.com/plugin/3847)

![截图](/images/java/preview_zps13c573f1.png)

### 支持类型

在Findbugs中能找到下面类型的bug(有些不算是bug，应该说是不好的实践)。

<table class="table table-bordered table-striped table-condensed">
	<tr>
		<th>Cateory</th>
		<th>Number</th>
		<th>Samples</th>
	</tr>
	<tr>
    <td>
        <p>Correctness</p></td>
    <td>
        <p>142</p></td>
    <td>
        <p>Illegal format string</p>

        <p>Null value is guaranteed to be dereferenced</p>

        <p>Call to equals() comparing unrelated class and interface</p></td>
</tr>
<tr>
    <td>
        <p>Bad practice</p></td>
    <td>
        <p>84</p></td>
    <td>
        <p>Confusing method names</p>

        <p>Method may fail to close stream </p>

        <p>Comparison of String parameter using == or !=</p></td>
</tr>
<tr>
    <td>
        <p>Dodgy code</p></td>
    <td>
        <p>71</p></td>
    <td>
        <p>Useless control flow</p>

        <p>Integer remainder modulo 1</p>

        <p>Redundant null check of value known to be null</p></td>
</tr>
<tr>
    <td>
        <p>Multithreaded Correctness</p></td>
    <td>
        <p>45</p></td>
    <td>
        <p>A thread was created using the default empty run method</p>

        <p>Class's writeObject() method is synchronized but nothing else is</p></td>
</tr>
<tr>
    <td>
        <p>Performance</p></td>
    <td>
        <p>27</p></td>
    <td>
        <p>Method concatenates strings using + in a loop</p>

        <p>Method invokes inefficient Boolean constructor</p></td>
</tr>
<tr>
    <td>
        <p>Malicious Code Vulnerability</p></td>
    <td>
        <p>15</p></td>
    <td>
        <p>Finalizer should be protected, not public</p>

        <p>Field isn't final and can't be protected from malicious code</p></td>
</tr>
<tr>
    <td>
        <p>Security</p></td>
    <td>
        <p>11</p></td>
    <td>
        <p>Hardcoded constant database password</p>

        <p>A prepared statement is generated from a variable String</p></td>
</tr>
<tr>
    <td>
        <p>Experimental</p></td>
    <td>
        <p>3</p></td>
    <td>
        <p>Method may fail to clean up stream or resource</p></td>
</tr>
<tr>
    <td>
        <p>Internationalization</p></td>
    <td>
        <p>2</p></td>
    <td>
        <p>Consider using Locale parameterized version of invoked method</p></td>
</tr>
	

</table>

### 发现代码中的问题

#### 性能：
低效的迭代器：
![/images/java/performance_1_zps684644d6.png](/images/java/performance_1_zps684644d6.png)

修改后：

    for (Integer groupKey : groupList.keySet()) 
        insertGroupedList(groupKey);
    }
    
装箱/拆箱
![/images/java/performance_2_zpsfa307a6d.png](/images/java/performance_2_zpsfa307a6d.png)
`0L`在java.lang.Long中是有缓存的，所以常量0L先拆箱成小long，然后赋值给agengId的时候装箱成大Long，两次操作就是没有必要的。

修改后：

    agentId = (agentId == null) ? Long.valueOf(0) : agentId;
    
静态变量:

![/images/java/performance_3_zps953e82e7.png](/images/java/performance_3_zps953e82e7.png)

这个常量是在初始化的时候赋值的，在内存中没有必要存在多份，可以声明成static。


内部类，何不静态？

![/images/java/performance_4_zps26c9d77b.png](/images/java/performance_4_zps26c9d77b.png)

MuFlightInfoDO是MuFlightFareQuery的内部类，MuFlightInfoDO的实例保存了一份创建它的类（MuFlightFareQuery）的引用，这样不仅使MuFlightInfoDO类占用更大的空间，而且是外部的MuFlightFareQuery生命周期更长了。

#### 不好的实践：

忽略异常：

![/images/java/bp_1_zps8cbf8e46.png](/images/java/bp_1_zps8cbf8e46.png)


集合使用removeAll来清理：

![/images/java/bp_2_zps02c6d731.png](/images/java/bp_2_zps02c6d731.png)

直接调用clear()或者是个bug?

流程：

![/images/java/bp_3_zps5fb05951.png](/images/java/bp_3_zps5fb05951.png)

else if里面有没任何代码，考虑删除。

集合：

![/images/java/bp_4_zps48cb693d.png](/images/java/bp_4_zps48cb693d.png)

`hsfServices` 声明成map：ConcurrentHashMap&lt;String,HSFSpringConsumerBean hsfServices&gt;

在这里hsfServices.contains(key)参数是String类型，但是声明的map是HSFSpringConsumerBean类型，应该永远不会返回true，应该是containsKey吧？
    
equal类型不匹配：

![/images/java/QQ622A56FE20131124134350_zps59c66a2c.png](/images/java/QQ622A56FE20131124134350_zps59c66a2c.png)

dfare.getAgentSupportType().equals(AgentSupportType.ONE) 左边Integer，右边enum。
    

#### 线程安全：

DateFormat

![/images/java/qq_zps9b7f1f9c.png](/images/java/qq_zps9b7f1f9c.png)
DateFormat不是线程安装的，在这里声明为类成员，会有问题。

### 总结
有些代码问题在人为的review的时候很难发现，如果上线了会有很大的隐患，虽然当时没有出现bug。利用Findbugs在提交代码的时候自己进行review。让你的代码出更少的bug。

> Keep the bar green, keep you code clean.    
  
    


