---
layout: post
title: "分享一个自己写的字符串工具：字符串格式化拼接"
description: ""
category: .net
subhead: ''
tags: [.net,  c#,  tool,  string]

---

在强类型的语言(java,C#)中，我们经常会拼接一些字符串。有的时候要拼接的字符串会很长，比如把一个网页的HTML代码拿出来放在一个变量里面，这时候拼接字符串很麻烦，要处理换行，单引号，双引号问题。

为了解决这个烦人的问题，写了一个字符串小工具：（界面佷虽 - -!）

![image](http://i1298.photobucket.com/albums/ag53/lichengwu/1_zps8304e64b.gif)

eg：如果有这样一行字符串Html代码  

    <body onload="alert('haha!')">   
    <table width="200" border="1">   
      <tr>   
        <td> </td>   
        <td> </td>   
      </tr>   
      <tr>   
        <td> </td>   
        <td> </td>   
      </tr>   
    </table>   
    </body>   
 
我们要把它变成字符串放到程序的变量中，手动转化很麻烦。
这个工具有三种模式，Single Line（A）：单行单个字符串
上面的eg代码转换后效果

![image](http://i1298.photobucket.com/albums/ag53/lichengwu/2_zpsace5b46f.gif)

也就是这样的
  
    "<body onload=/"alert(/'haha!/')/"> <table width=/"200/" border=/"1/"> <tr> <td> </td> <td> </td> </tr> <tr> <td> </td> <td> </td> </tr> </table> </body>"  
 
呵呵 一个字符串了。
Single Line(M):单行多字符串拼接模式
转换后的效果：
  
    "<body onload=/"alert(/'haha!/')/"> "+"<table width=/"200/" border=/"1/"> "+"<tr> "+"<td> </td> "+"<td> </td> "+"</tr> "+"<tr> "+"<td> </td> "+"<td> </td> "+"</tr> "+"</table> "+"</body>"  
 
Multi Lines:多行多字符串拼接
效果如下：
  
    "<body onload=/"alert(/'haha!/')/"> "  
    +"<table width=/"200/" border=/"1/"> "  
    +"<tr> "  
    +"<td> </td> "  
    +"<td> </td> "  
    +"</tr> "  
    +"<tr> "  
    +"<td> </td> "  
    +"<td> </td> "  
    +"</tr> "  
    +"</table> "  
    +"</body>"  
 
说明：目前测试转换html没问题，其他的字符串没测试。本程序基于.net 2.0 环境。不能运行很可能是没有.net 2.0 环境。
下一个版本加入去除注释（java注释，html注释，xml注释等）的功能。
程序及源码下载 [http://cid-2c8a0dc7c1eb1d71.skydrive.live.com/self.aspx/soft/MakeAString.7z](http://cid-2c8a0dc7c1eb1d71.skydrive.live.com/self.aspx/soft/MakeAString.7z) (用IE打开，这个不是文件地址)

{% include JB/setup %}