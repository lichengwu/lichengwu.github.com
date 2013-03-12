---
layout: post
title: "利用反射访问类的私有成员"
description: ""
category: java
subhead: ''
tags: [java, reflection]

---

一般情况下，java类的私有成员变量不能直接访问，如果想要访问某个私有成员变量，就要给这个变量写一个访问方法getXXX()。

如果累没有定义这个访问方法，我们好像束手无策的。

其实，利用java的反射机制，我们可以做到！

    public class AccessPrivateField {  
        @SuppressWarnings("unused")  
        private String privateField = "private";  
  
        @SuppressWarnings("unchecked")  
        public static void main(String[] args) {  
            try {  
                Class cls = Class.forName("cdsn.test.oliver.javase.AccessPrivateField");  
                Object obj = cls.newInstance();  
                Field field = cls.getDeclaredField("privateField");  
                field.setAccessible(true);  
                System.out.println(field.get(obj));  
            } catch (Exception e) {  
                e.printStackTrace();  
            }  
        }  
    }  
 
输出结果：`private`

{% include JB/setup %}