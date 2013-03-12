---
layout: post
title: "java泛型实现原理"
description: ""
category: java
subhead: ''
tags: [java, generic-type]

---

DK1.5增加的新特性里面有一个就是泛型。对于泛型的评价，褒贬不一，废话不多说，先来看看他的原理。

泛型是提供给javac编译器使用的，可以限定集合中的输入类型，让编译器拦截源程序中的非法输入，编译器编译带类型说明的

集合时会去掉类型信息，对于参数化得泛型类型，getClass()方法的返回值和原始类型完全一样。

对于下面这个源程序： 
 
    public class Oliver {  
        public static void main(String[] args) {  
            ArrayList<String> list = new ArrayList<String>();  
            list.add("str1");  
            list.add("str2");  
            String str = list.get(0);  
        }  
    }  
 
编译成Oliver.class后反编译的内容：
   
    public class Oliver {  
        public Oliver() {  
        }  
  
        public static void main(String[] args) {  
            ArrayList list = new ArrayList();  
            list.add("str1");  
            list.add("str2");  
            String str = (String) list.get(0);  
        }  
    }  
 
也就是说java的泛型只是在编译器做了参数限制，其实对性能并没有什么优化！

由于编译生成的字节码会去掉泛型的类型信息，只要能跳过编译器，就可以往某个泛型集合中加入其它的类型数据。
 
下面代码展示利用反射机制跳过编译器检查： 

    public class Oliver {  
        public static void main(String[] args) throws Exception {  
            ArrayList<Integer> list = new ArrayList<Integer>();  
            list.getClass().getMethod("add", Object.class).invoke(list, "ssss");  
            System.out.println("list:" + list.get(0));  
        }  
    }
      
输出结果：
 
    list:ssss
 
对于java泛型，Bruce Ecke（Thinking in Java作者）曾经给出这样的评论： 

>Guess what. I really don't care. You want to call it "generics," fine, implement something that looks like C++ or Ada, that actually produces a latent typing mechanism like they do. But don't implement something whose sole purpose is to solve the casting problem in containers, and then insist that on calling it "Generics." Of course, Java has long precedence in arrogantly mangling well- accepted meanings for things: one that particularly stuck in my craw was the use of "design pattern" to describe getters and setters. In JDK 1.4 we were told some lame way to use assertions which was a backward justification for them being disabled by default. JDK 1.4 also had to invent its own inferior logging system rather than using the openly created, well-tested and well-liked Log4J. And we've also been told many times about how Java has been as fast or faster than C++, or about how one or another feature is great and flawless. I point to the threads implementation which has had major changes quietly made from version to version with not so much as a peep of apology or admission that "hey, we really screwed up here." Or maybe I was just not on that particular announcement list.


{% include JB/setup %}