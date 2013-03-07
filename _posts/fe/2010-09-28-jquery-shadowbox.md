---
layout: post
title: "jQuery插件shadowbox体验"
description: ""
category: 前端
subhead: ''
tags: [shadowbox,  jquery,  web,  javascript]

---

以前用的图片展示用的是mootools的一个工具，感觉不错，不过mootools跟jquery有冲突，所以找了个jquery的插件，感觉还不错。
shadowbox支持图片，视频，html，frame，ajax还是很强大的。
根据官网demo，写了几个例子：

#### 1.简单的图片展示
 
    <html xmlns="http://www.w3.org/1999/xhtml">
    <head>
        <meta http-equiv="Content-Type" content="text/html; charset=utf-8"/>
        <title>New Web Project</title>
        <link rel="stylesheet" type="text/css" href="js/shadowbox-3.0.3/shadowbox.css" />
        <script type="text/javascript" src="js/jquery/jquery-1.4.2.js" src="js/jquery/jquery-1.4.2.js"></script>
        <script type="text/javascript" src="js/shadowbox-3.0.3/shadowbox.js" rc="js/shadowbox-3.0.3/shadowbox.js"></script>
        <script type = "text/javascript" >
                //初始化     
                Shadowbox.init({});
        </script>
    </head>
    <body>
        <!--rel="shadowbox" 关联shadowbox-->
        <a href="images/1.jpg" _href="images/1.jpg" rel="shadowbox">图片1</a>
        <a href="images/2.jpg" _href="images/2.jpg" rel="shadowbox">图片2</a>
        <a href="images/3.jpg" _href="images/3.jpg" rel="shadowbox">图片3</a>
        <a href="images/4.jpg" _href="images/4.jpg" rel="shadowbox">图片4</a>
        <a href="images/5.jpg" _href="images/5.jpg" rel="shadowbox">图片5</a>
    </body>
    </html> 
 
#### 2.一些参数设置

    <html xmlns="http://www.w3.org/1999/xhtml">
    <head>
        <meta http-equiv="Content-Type" content="text/html; charset=utf-8"/>
        <title>New Web Project</title>
        <link rel="stylesheet" type="text/css" href="js/shadowbox-3.0.3/shadowbox.css"
              mce_href="js/shadowbox-3.0.3/shadowbox.css">
        <script type="text/javascript" src="js/jquery/jquery-1.4.2.js" mce_src="js/jquery/jquery-1.4.2.js"></script>
        <script type="text/javascript" src="js/shadowbox-3.0.3/shadowbox.js"
                src="js/shadowbox-3.0.3/shadowbox.js"></script>
        <script type="text/javascript"><!--
        Shadowbox.init({
            handleOversize: "drag",//当图片过大的时候，可以腿拽  
            //handleOversize: "none",//当图片过大的时候，不显示多余的部分  
            //handleOversize: "resize",//默认:自动调整图片大小  
            modal: true//Set this true to prevent mouse clicks on the overlay from closing Shadowbox. Defaults to false.  
            //true:鼠标点击非图片区域无反应 false:鼠标点击非图片区域图片效果消失  
        });
        // --></script>
    </head>
    <body>
        <!--设置标题-->
        <a href="images/1.jpg" mce_href="images/1.jpg" rel="shadowbox" title="图片1">图片1</a>
        <a href="images/2.jpg" mce_href="images/2.jpg" rel="shadowbox" title="图片2">图片2</a>
        <!--设置显示的大小-->
        <a href="images/3.jpg" mce_href="images/3.jpg" rel="shadowbox;height=200;width=320">图片3</a>
        <a href="images/4.jpg" mce_href="images/4.jpg" rel="shadowbox">图片4</a>
        <a href="images/5.jpg" mce_href="images/5.jpg" rel="shadowbox">图片5</a>
    </body>
    </html> 

 
#### 3.图片组，幻灯片效果

    <html xmlns="http://www.w3.org/1999/xhtml">
    <head>
        <meta http-equiv="Content-Type" content="text/html; charset=utf-8"/>
        <title>New Web Project</title>
        <link rel="stylesheet" type="text/css" href="js/shadowbox-3.0.3/shadowbox.css"
              mce_href="js/shadowbox-3.0.3/shadowbox.css">
        <script type="text/javascript" src="js/jquery/jquery-1.4.2.js" src="js/jquery/jquery-1.4.2.js"></script>
        <script type="text/javascript" src="js/shadowbox-3.0.3/shadowbox.js"
                src="js/shadowbox-3.0.3/shadowbox.js"></script>
        <script type="text/javascript"><!--
        Shadowbox.init({
        });
        // --></script>
    </head>
    <body>
    <!--shadowbox[Vacation] 设置图片组，幻灯片效果-->
    <a href="images/1.jpg" mce_href="images/1.jpg" rel="shadowbox[Vacation]">图片1</a>
    <a href="images/2.jpg" mce_href="images/2.jpg" rel="shadowbox[Vacation]">图片2</a>
    <a href="images/3.jpg" mce_href="images/3.jpg" rel="shadowbox[Vacation]">图片3</a>
    <a href="images/4.jpg" mce_href="images/4.jpg" rel="shadowbox[Vacation]">图片4</a>
    <a href="images/5.jpg" mce_href="images/5.jpg" rel="shadowbox[Vacation]">图片5</a>
    </body>
    </html>

  
#### 4.视频(在线视频地址选择flv的地址，或者swf的)


    <html xmlns="http://www.w3.org/1999/xhtml">
    <head>
        <meta http-equiv="Content-Type" content="text/html; charset=utf-8"/>
        <title>New Web Project</title>
        <link rel="stylesheet" type="text/css" href="js/shadowbox-3.0.3/shadowbox.css"
              mce_href="js/shadowbox-3.0.3/shadowbox.css">
        <script type="text/javascript" src="js/jquery/jquery-1.4.2.js" mce_src="js/jquery/jquery-1.4.2.js"></script>
        <script type="text/javascript" src="js/shadowbox-3.0.3/shadowbox.js"
                src="js/shadowbox-3.0.3/shadowbox.js"></script>
        <script type="text/javascript"><!--
        Shadowbox.init({});
        // --></script>
    </head>
    <body>
    <a href="videos/sample.flv" mce_href="videos/sample.flv" rel="shadowbox;width=320;height=240">本地flv视频</a>
    <a href="videos/sample.mov" mce_href="videos/sample.mov" rel="shadowbox;width=320;height=240">本地mov视频</a>
    <a href="videos/sample.mp4" mce_href="videos/sample.mp4" rel="shadowbox;width=320;height=240">本地mp4视频</a>
    <a href="http://player.youku.com/player.php/sid/XMjA5Nzg1ODM2/v.swf"
       mce_href="http://player.youku.com/player.php/sid/XMjA5Nzg1ODM2/v.swf" rel="shadowbox;width=480;height=400">优酷视频</a>
    <a href="http://www.tudou.com/l/Ezhl3cOwdBs" mce_href="http://www.tudou.com/l/Ezhl3cOwdBs"
       rel="shadowbox;width=320;height=240">土豆视频</a>
    </body>
    </html>  

#### 5.混合，幻灯片效果，有图片，视频，html

    <html xmlns="http://www.w3.org/1999/xhtml">
    <head>
        <meta http-equiv="Content-Type" content="text/html; charset=utf-8"/>
        <title>New Web Project</title>
        <link rel="stylesheet" type="text/css" href="js/shadowbox-3.0.3/shadowbox.css"
              mce_href="js/shadowbox-3.0.3/shadowbox.css">
        <script type="text/javascript" src="js/jquery/jquery-1.4.2.js" src="js/jquery/jquery-1.4.2.js"></script>
        <script type="text/javascript" src="js/shadowbox-3.0.3/shadowbox.js"
                src="js/shadowbox-3.0.3/shadowbox.js"></script>
        <script type="text/javascript"><!--
        Shadowbox.init({
        });
        // --></script>
    </head>
    <body>
    <a href="images/1.jpg" mce_href="images/1.jpg" rel="shadowbox[Mixed]">图片1</a>
    <a href="images/2.jpg" mce_href="images/2.jpg" rel="shadowbox[Mixed]">图片2</a>
    <a href="videos/sample.flv" mce_href="videos/sample.flv" rel="shadowbox[Mixed];width=320;height=240">本地flv视频</a>
    <a href="http://player.youku.com/player.php/sid/XMjA5Nzg1ODM2/v.swf"
       mce_href="http://player.youku.com/player.php/sid/XMjA5Nzg1ODM2/v.swf" rel="shadowbox[Mixed];width=480;height=400">优酷视频</a>
    <a href="http://www.baidu.com" mce_href="http://www.baidu.com" rel="shadowbox[Mixed]">优酷视频</a>
    </body>
    </html>  


还有就是ajax的，这个是高级功能，首先要了解shadowbox的函数，才能写出来
本人技术有限，等学好jQuery和ajax再研究。

总的来说还不错，够用了。

{% include JB/setup %}