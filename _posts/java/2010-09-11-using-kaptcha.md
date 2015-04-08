---
layout: post
title: "kaptcha 简单方便的验证码生成工具"
description: ""
category: java
subhead: ''
tags: [kaptcha, captcha, java]

---

kaptcha是一个非常实用的验证码生成工具，有了它，你可以生成各种样式的验证码，因为它是可配置的。

kaptcha工作的原理是调用com.google.code.kaptcha.servlet.KaptchaServlet，生成一个图片。同时将生成的验证码字符串放到HttpSession中。

#### kaptcha可以配置一下信息：

##### 1.验证码的字体

##### 2.验证码字体的大小

##### 3.验证码字体的字体颜色

##### 4.验证码内容的范围(数字，字母，中文汉字！)

##### 5.验证码图片的大小，边框，边框粗细，边框颜色

##### 6.验证码的干扰线(可以自己继承com.google.code.kaptcha.NoiseProducer写一个自定义的干扰线)

##### 7.验证码的样式(鱼眼样式、3D、普通模糊……当然也可以继承com.google.code.kaptcha.GimpyEngine自定义样式)

##### 8.……

详细信息请看下面的web.xml文件

#### 下面介绍一下用法:

##### 1. 首先去官网下载jar:http://code.google.com/p/kaptcha/

##### 2. 建立一个web项目，导入kaptcha-2.3.jar到环境变量中。

##### 3. 配置web.xml文件

其实就是配置com.google.code.kaptcha.servlet.KaptchaServlet

    <?xml version="1.0" encoding="UTF-8"?>
    <web-app xmlns="http://java.sun.com/xml/ns/j2ee" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="http://java.sun.com/xml/ns/j2ee http://java.sun.com/xml/ns/j2ee/web-app_2_4.xsd"
             version="2.4">
        <servlet>
            <servlet-name>Kaptcha</servlet-name>
            <servlet-class>com.google.code.kaptcha.servlet.KaptchaServlet</servlet-class>
            <init-param>
                <description>Border around kaptcha. Legal values are yes or no.</description>
                <param-name>kaptcha.border</param-name>
                <param-value>no</param-value>
            </init-param>
            <init-param>
                <description>Color of the border. Legal values are r,g,b (and
                    optional alpha) or white,black,blue.
                </description>
                <param-name>kaptcha.border.color</param-name>
                <param-value>red</param-value>
            </init-param>
            <init-param>
                <description>Thickness of the border around kaptcha. Legal values are
                    > 0.
                </description>
                <param-name>kaptcha.border.thickness</param-name>
                <param-value>5</param-value>
            </init-param>
            <init-param>
                <description>Width in pixels of the kaptcha image.</description>
                <param-name>kaptcha.image.width</param-name>
                <param-value>80</param-value>
            </init-param>
            <init-param>
                <description>Height in pixels of the kaptcha image.</description>
                <param-name>kaptcha.image.height</param-name>
                <param-value>40</param-value>
            </init-param>
            <init-param>
                <description>The image producer.</description>
                <param-name>kaptcha.producer.impl</param-name>
                <param-value>com.google.code.kaptcha.impl.DefaultKaptcha</param-value>
            </init-param>
            <init-param>
                <description>The text producer.</description>
                <param-name>kaptcha.textproducer.impl</param-name>
                <param-value>com.google.code.kaptcha.text.impl.DefaultTextCreator</param-value>
            </init-param>
            <init-param>
                <description>The characters that will create the kaptcha.</description>
                <param-name>kaptcha.textproducer.char.string</param-name>
                <param-value>abcde2345678gfynmnpwx</param-value>
            </init-param>
            <init-param>
                <description>The number of characters to display.</description>
                <param-name>kaptcha.textproducer.char.length</param-name>
                <param-value>5</param-value>
            </init-param>
            <init-param>
                <description>A list of comma separated font names.</description>
                <param-name>kaptcha.textproducer.font.names</param-name>
                <param-value>Arial, Courier</param-value>
            </init-param>
            <init-param>
                <description>The size of the font to use.</description>
                <param-name>kaptcha.textproducer.font.size</param-name>
                <param-value>23</param-value>
            </init-param>
            <init-param>
                <description>The color to use for the font. Legal values are r,g,b.</description>
                <param-name>kaptcha.textproducer.font.color</param-name>
                <param-value>black</param-value>
            </init-param>
            <init-param>
                <description>The noise producer.</description>
                <param-name>kaptcha.noise.impl</param-name>
                <param-value>com.google.code.kaptcha.impl.NoNoise</param-value>
            </init-param>
            <init-param>
                <description>The noise color. Legal values are r,g,b.</description>
                <param-name>kaptcha.noise.color</param-name>
                <param-value>black</param-value>
            </init-param>
            <init-param>
                <description>The obscurificator implementation.</description>
                <param-name>kaptcha.obscurificator.impl</param-name>
                <param-value>com.google.code.kaptcha.impl.ShadowGimpy</param-value>
            </init-param>
            <init-param>
                <description>The background implementation.</description>
                <param-name>kaptcha.background.impl</param-name>
                <param-value>com.google.code.kaptcha.impl.DefaultBackground</param-value>
            </init-param>
            <init-param>
                <description>Ending background color. Legal values are r,g,b.</description>
                <param-name>kaptcha.background.clear.to</param-name>
                <param-value>white</param-value>
            </init-param>
            <init-param>
                <description>The word renderer implementation.</description>
                <param-name>kaptcha.word.impl</param-name>
                <param-value>com.google.code.kaptcha.text.impl.DefaultWordRenderer</param-value>
            </init-param>
            <init-param>
                <description>The value for the kaptcha is generated and is put into
                    the HttpSession. This is the key value for that item in the session.
                </description>
                <param-name>kaptcha.session.key</param-name>
                <param-value>KAPTCHA_SESSION_KEY</param-value>
            </init-param>
            <init-param>
                <description>The date the kaptcha is generated is put into the
                    HttpSession. This is the key value for that item in the session.
                </description>
                <param-name>kaptcha.session.date</param-name>
                <param-value>KAPTCHA_SESSION_DATE</param-value>
            </init-param>
        </servlet>
        <servlet-mapping>
            <servlet-name>Kaptcha</servlet-name>
            <url-pattern>/Kaptcha.jpg</url-pattern>
        </servlet-mapping>
        <welcome-file-list>
            <welcome-file>KaptchaExample.jsp</welcome-file>
        </welcome-file-list>
    </web-app>   

   
 
编写KaptchaExample.jsp
这里用到了jQuery，所以添加了jQuery的库


    <img src="Kaptcha.jpg" mce_src="Kaptcha.jpg" id="kaptchaImage"/>
    <script type="text/javascript">
        $('#kaptchaImage').click(
            function () {
                $(this).hide().attr('src','Kaptcha.jpg?' + Math.floor(Math.random() **100)).fadeIn();
        })
    </script>
    <%
        String c = (String) session
                .getAttribute(com.google.code.kaptcha.Constants.KAPTCHA_SESSION_KEY);
        String parm = (String) request.getParameter("kaptchafield");
    
        System.out.println(c);
        out.println("Parameter: " + parm + " ? Session Key: " + c + " : ");
        if (c != null && parm != null) {
            if (c.equals(parm)) {
                out.println("<b>true</b>");
            } else {
                out.println("<b>false</b>");
            }
        }
    %>
 
运行测试
 
工程下载：[http://download.csdn.net/source/2687960](http://download.csdn.net/source/2687960)


