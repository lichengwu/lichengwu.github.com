---
layout: post
title: "CodeSmith教程"
description: ""
category: c-sharp
subhead: ''
tags: [.net,  codesmith, c#]

---

最好的方法了解CodeSmith是进行尝试。你可以试着使用CodeSmith，在你还没有了解他的全部特性之前。

在本节中，您将学习如何使用CodeSmith中产生有用的实用程序代码一块-特别是强类型的哈希表类。这项工作应该采取你不超过五分钟才能完成，但会向您介绍`CodeSmith中Explorer`和`CodeSmith Studio`，并显示你的CodeSmith中的基于模板的代码生成方案的权力。

一种方法启动代码生成会话是CodeSmith中资源管理器。正如Windows资源管理服务组织文件和文件夹在计算机上存储，CodeSmith资源服务组织模板。为了从CodeSmith启动程序菜单CodeSmith中资源管理器，选择CodeSmith中资源管理器。他将显示你已安装的CodeSmith模板。

![](/images/c-sharp/1_zps0cd3315a.png)

模板是生成代码的一部分，你可以使用Codesmith的模板生成代码，或者用CodeSmith开发自己的模板。`.cst`文件扩展名代表“CodeSmith中模板”例如，CSHashTable.cst模板生成哈希表类的C＃代码。

双击此模板（或右键单击并选择执行）将其打开。

![](/images/c-sharp/2_zps758f4836.png)

CodeSmith模板使用属性，让您自定义生成的代码。当您打开一个模板CodeSmith中资源管理器，该模板的属性表显示的所有属性。您需要提供这些属性的值，之后CodeSmith中可以为您生成代码。该CSHashTable模板需要四个字符串属性（所属类别，ClassNamespace，ItemType值和KeyType）和一个枚举属性。您可以键入任何值为字符串属性一样。首次实验，填写属性表是这样的：单击生成按钮，右边显示了根据模板和属性生成的代码。

![](/images/c-sharp/3_zpsf5670d6d.png)

把代码粘贴到你的文件中，或者单击保存按钮保存它。	

