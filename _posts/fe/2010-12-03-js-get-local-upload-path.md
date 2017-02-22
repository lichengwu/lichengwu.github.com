---
layout: post
catalog: true
title: "JavaScript 获取上传文件的本地绝对路径"
description: ""
category: fe
subhead: ''
tags: [javascript]

---
一直苦恼于在表单提交时获得上传文件的本地绝对路径。

由于javascript是在浏览器环境运行的脚本语言，所以javascript的权限很低，不能操作本地资源，这样的好处是安全性提高了，但是也带来了开发的不便。

其实这里介绍的获取文件路径也是一个破坏安全性的例子，不过有的时候确实有这方面的需求。

本人测试，在chrome 8，firefox3.6，IE9下能正常运行，下面是代码：
 
    try {  
        netscape.security.PrivilegeManager.enablePrivilege( 'UniversalFileRead' );  
    }  
    catch (err) {  
        //need to set signed.applets.codebase_principal_support to true  
    }  
 
PS：在firefox中，需要设置：
打开"about:config"页面，查找`signed.applets.codebase_principal_support`属性，将其值设置为`true`。

