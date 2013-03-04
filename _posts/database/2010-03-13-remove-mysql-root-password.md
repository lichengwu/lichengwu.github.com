---
layout: post
title: "删除MySQL的root密码的方法"
description: ""
category: database
subhead: ''
tags: [mysql,  password, database]

---
在mysql>下输入以下2行:

    GRANT USAGE ON *.* TO root@localhost IDENTIFIED BY ' ' ; 
    FLUSH PRIVILEGES; 


{% include JB/setup %}
