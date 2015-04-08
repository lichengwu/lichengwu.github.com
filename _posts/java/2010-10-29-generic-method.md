---
layout: post
title: "参数化方法返回类型，参数化数组"
description: ""
category: java
subhead: ''
tags: [java, generic-type]

---

有的时候，我们想让一个方法根据参数类型返回相应类型的值，还有的时候，想把数组参数话，今天在Think In Java看到了一小段有用的代码，分享！
可以采用两种方式实现：类参数和方法参数，个人觉得方法参数比较好，灵活性大。  
 
    class ClassParameter<T>{  
        public T[] func(T[] arg){return arg;}  
    }  
    class MethodParameter{  
        public static<T> T[] func(T[] arg){return arg;}  
    }  
    public class ParameterizedArrayType{  
        public static void main(String[] args) {  
            //实例化ClassParameter  
            ClassParameter<Integer> cp = new ClassParameter<Integer>();  
            Integer[] int1 = {1,2,3,4,5};  
            //通过类参数来创建数组  
            Integer[] int2=cp.func(int1);  
            //通过方法参数来创建数组  
            Integer[] int3=MethodParameter.func(int1);  
            //打印结果  
            System.out.println(Arrays.toString(int2));  
            System.out.println(Arrays.toString(int3));  
        }  
    }  
 

