---
layout: post
title: "JSP 分页框架 Pager Tag Library使用"
description: ""
category: java
subhead: ''
tags: [pager, java]

---

很久没写技术文章了，最近正在学习，把以前的东西总结一下。

最新的Pager版本请到[http://jsptags.com/tags/navigation/pager/](http://jsptags.com/tags/navigation/pager/) 下载

Pager采用标签的形式对数据进行分页，用法简单，支持自定义分页样式。

举个例子,效果如下:

![image](/images/java/1_zpsbb69f644.png)


注：demo采用了SSH（struts2+hibernate+spring) 其实根本不用这么复杂就能演示，只是为了学习，分页效果加入css样式。

1. 首先下载pager-taglib.jar并放到classpath中。
2. 写好分页查询的后台方法，将数据返回给jsp页面。
3. jsp页面抓去数据，并显示：(需要的数据：查询返回的当前页数据集合、当前查询返回的总条数)。

![image](/images/java/2_zps1384fc77.png)

#### 标签说明：

##### pg:pager 
定义分页开始标签，在这个标签里面可以定义pager:index pager:next……相当于html的body标签 

#### 格式： 
> id="value"//id默认为pager，用来获得pager的属性。例如：request.getParamenter(“pager.offset”);
> url="url"//用来生成每次分页请求的url，默认request.getRequestURI()，如果默认，所以url参数将被移除       
> index="center|forward|half-full"//页数显示的风格       
> maxPageItems="int"//每页显示数据的条数       
> maxIndexPages="int"//分页条显示的页数       
> scope="page|request"//pager对象的scope       

#### pg:param 
相当于request.setAttribute(“name”,"value“),分页url传参 

#### 格式： 

>id="value" 
>name="value" 
>value="value" 

#### pg:index 
分页索引，`定义在pg:pager里面` 

#### 格式： 

>id="value" 
>export="expression" 
 

#### pg:first 
用在`pg:index`里面，生成首页链接 

#### 格式： 

>id="value" 
>unless="current|indexed"//首页显示 current：当前页是首页的时候就不显示首页链接 indexed：只要页码里面有首页就不显示首页链接 
>export="expression" 
><%= pageUrl %>//url链接 
><%= pageNumber %>//页数 

#### pg:prev 
用在pg:index里面，生成上一页链接 

#### 格式： 

>id="value" 
>ifnull="true|false"//是否总是显示上一页链接，没有上一页的时候也显示！ 
>export="expression" 
><%= pageUrl %> 
><%= pageNumber %> 

#### pg:page 
用在pg:index里面，生成当前页码链接或者导出当前页 

#### 格式： 

>id="value" 
>export="expression" 
><%= pageUrl %> 
><%= pageNumber %> 

#### pg:pages 
用在pg:index里面，生成所有页码 

#### 格式： 

>id="value" 
>export="expression" 
><%= pageUrl %> 
><%= pageNumber %> 

#### pg:next 
用在pg:index里面，用来生成下一页链接 

#### 格式： 

>id="value" 
>ifnull="true|false" 
>export="expression" 
><%= pageUrl %> 
><%= pageNumber %> 

#### pg:last 
用在pg:index里面，用来生成尾页链接 

#### 格式： 

>id="value" 
>unless="current|indexed" 
>export="expression" 
><%= pageUrl %> 
><%= pageNumber %> 

