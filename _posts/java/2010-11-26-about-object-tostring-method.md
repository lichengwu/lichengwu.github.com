---
layout: post
title: "Object.toString()返回字符串的意义：对象名+@+对象内存地址？"
description: ""
category: java
subhead: ''
tags: [java]

---

在java中，如果一个对象未重写toString()方法，那么它将会调用父类的toString()，如果父类也没有重写这个方法，那么就迭代往上调用，直到Object的toString()方法。

我们在打印这个toStirng()方法的时候，会出现XXXX@e29820字样，那么@后面的值到底是什么呢，它是对象所在的内存地址么？下面我们来证明：

    public class ObjectToStringTest {
        /**
         * 默认值。
         */
        private static final int SIZE = 10000;

        public static void main(String[] args) {
            // 创建列表存放对象
            List<Object> list = new ArrayList<Object>();
            int existNumber = 0;
            // 新建SIZE个对象，如果toStirng代表的是内存地址，地址是不会重复的，
            // 那么list中应该不会存在重复的元素。
            // list的大小应该为SIZE
            for (int i = 0; i < SIZE; i++) {
                Object obj = new Object();
                if (list.contains(obj.toString())) {
                    System.out.println("对象：" + obj.toString() + "已存在!");
                    existNumber++;
                } else
                    list.add(obj.toString());
            }
            System.out.println("列表List的大小:" + list.size());
            System.out.println("重复元素的个数：" + existNumber);
            System.out.println("===============华丽的分割线===============");
            // 清空list
            list.clear();
            existNumber = 0;
            // 新建一个对象的时候，变量名是对这个对象的应用（相当于对象的"地址"）
            // 利用这个原理，我们再测试
            for (int i = 0; i < SIZE; i++) {
                Object obj = new Object();
                if (list.contains(obj)) {
                    System.out.println("对象：" + obj + "已存在!");
                    existNumber++;
                } else
                    list.add(obj.toString());
            }
            System.out.println("列表List的大小:" + list.size());
            System.out.println("重复元素的个数：" + existNumber);
        }
    }
 
运行结果如下：
>对象：java.lang.Object@922804已存在!

>对象：java.lang.Object@e29820已存在!

>列表List的大小:9998

>重复元素的个数：2

>\===============华丽的分割线===============

>列表List的大小:10000

>重复元素的个数：0

查看Object源代码： 
 
    public String toString() {  
        return getClass().getName() + "@" + Integer.toHexString(hashCode());  
    }  
 
原来返回的是hashCode，而hashCode算法可能出现hash冲突，所以上面的重复情况了。

PS：`如果没有出现重复情况，可以增大常量SIZE`。


{% include JB/setup %}