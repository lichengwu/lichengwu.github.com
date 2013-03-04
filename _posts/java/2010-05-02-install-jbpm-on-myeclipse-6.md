---
layout: post
title: "MyEclipse 6.5 JPBM 插件安装"
description: ""
category: java
subhead: ''
tags: [eclipse, jbpm, java]

---

实验了好几次，只有一种最不好的方法成功了。

### 第一种(**失败** )：官网给的方法

The installation of the GPD uses the Eclipse Software Update mechanism
and is pretty straightforward. There is an archived update site included
in the runtime installation of jBPM when you unzip it at
`install/src/gpd/jbpm-gpd-site.zip`

To add the update site to eclipse:

-   Help --\> Install New Software...
-   Click Add...
-   In dialog Add Site dialog, click Archive...
-   Navigate to `install/src/gpd/jbpm-gpd-site.zip` and click 'Open'
-   Clicking OK in the Add Site dialog will bring you back to the
    dialog 'Install'
-   Select the jPDL 4 GPD Update Site that has appeared
-   Click Next... and then Finish
-   Approve the license
-   Restart eclipse when that is asked
-   上面的官网的描述，具体做法如下图
    
    ![](http://i1298.photobucket.com/albums/ag53/lichengwu/1_zpsf9fb4e26.gif)
     
    ![](http://i1298.photobucket.com/albums/ag53/lichengwu/2_zps7ada51af.gif)
     

选择 new Archived site

![](http://i1298.photobucket.com/albums/ag53/lichengwu/3_zpsf9b2b3dc.gif)

ok之后finish 然后出现错误，实验了好几次，这个方法都没成功。

### 方法二(**失败** )：解压

将这个文件jbpm-jpdl-designer-site.zip解压 然后再方法一的红字选择New
Local Site 其他步骤一样，最后结果一样失败！

### 方法三(**成功** )：直接复制

将jbpm-jpdl-designer-site.zip解压后的文件复制到MyEclipse/eclipse目录，选择覆盖，重启Eclipse后，成功了
呵呵

![](http://i1298.photobucket.com/albums/ag53/lichengwu/4_zps247f9dcb.gif)

### 总结:
当我们安装Eclipse/MyEclipse插件的时候，一般都是用方法一和方法二(还有link的方法)，第三种方法是在是迫不得已，建议不要采用！

{% include JB/setup %}