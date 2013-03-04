---
layout: post
title: "初学汇编：MASMPlus下自定义Debug工具"
description: ""
category: assembly
subhead: ''
tags: [assembly, debug]

---

最近开始学汇编了，因为觉得高级语言有的时候也不是那么“高级”。

在用到MASMPlus这个工具时，发现没有Debug功能，所以利用自定义工具做了一个。步骤如下：

1.打开MASMPlus的配置


![](http://i1298.photobucket.com/albums/ag53/lichengwu/1_zps8016afdf.png)

2.在工具中添加一个自定义工具 

工具路径：C:/WINDOWS/system32/debug.exe
） 
 运行参数：\$(FileDir)/\$(FileName).exe 
 其他的随便写就可以了，然后确定保存。


![](http://i1298.photobucket.com/albums/ag53/lichengwu/2_zpsb5f03824.png)

3.在主界面会发现刚才自定义的工具


![](http://i1298.photobucket.com/albums/ag53/lichengwu/3_zps0a41321b.png)

这样工具就自定义完成了。

####注意：
*使用Debug时确保你的汇编代码没有语法错误并且已经编译和链接好了。*

{% include JB/setup %}
