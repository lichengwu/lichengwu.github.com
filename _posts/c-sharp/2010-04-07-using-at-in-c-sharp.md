---
layout: post
title: "C# @符号的多种使用方法"
description: ""
category: c#
subhead: ''
tags: [.net, c#]

---

#### 1. 限定字符串
用 `@` 符号加在字符串前面表示其中的转义字符“不”被处理。 
如果我们写一个文件的路径，例如"D:/文本文件"路径下的text.txt文件，不加@符号的话写法如下：

    stringfileName="D://文本文件//text.txt";

如果使用@符号就会比较简单：

    stringfileName=@"D:/文本文件/text.txt";
    
#### 2.让字符串跨行
有时候一个字符串写在一行中会很长(比如SQL语句)，不使用@符号，一种写法是这样的：

    string strSQL="SELECT * FROM HumanResources.Employee AS e"   
    +"INNER JOINPerson.Contact AS c"   
    +"ON e.ContactID=c.ContactID"   
    +"ORDERBY c.LastName";   
 

加上@符号后就可以直接换行了：
  
    string strSQL=@"SELECT * FROM HumanResources.Employee AS e INNER JOIN Person.Contact AS c ON e.ContactID=c.ContactID ORDERBYc.LastName";   
 
#### 3.在标识符中的用法
C#是不允许关键字作为标识符(类名、变量名、方法名、表空间名等)使用的，但如果加上@之后就可以了，例如：
  
    namespace @namespace   
    {   
        class @class   
         {   
            public static void @static(int @int)   
             {   
                if (@int > 0)   
                 {   
                     System.Console.WriteLine("Positive Integer");   
                 }   
                else if (@int == 0)   
                 {   
                     System.Console.WriteLine("Zero");   
                 }   
                else   
                 {   
                     System.Console.WriteLine("Negative Integer");   
                 }   
             }   
         }   
    }   
 



{% include JB/setup %}