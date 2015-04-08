---
layout: post
title: "Java中的transient，volatile和strictfp关键字"
description: ""
category: java
subhead: ''
tags: [java, transient, strictfp, volatile]

---

如果用`transient`声明一个实例变量，当对象存储时，它的值不需要维持。例如:


    class T{  
        transientint a;//不需要维持  
        int b;//需要维持  
    }  

这里，如果T类的一个对象写入一个持久的存储区域，a的内容不被保存，但b的将被保存。

`volatile`修饰符告诉编译器被volatile修饰的变量可以被程序的其他部分改变。在多线程程序中，有时两个或更多的线程共享一个相同的实例变量。考虑效率问题，每个线程可以自己保存该共享变量的私有拷贝。实际的变量副本在不同的时候更新，如当进入synchronized方法时。

2012-07-02 [https://gist.github.com/3033833](https://gist.github.com/3033833)   volatile 的一个小测试。 

用`strictfp`修饰类或方法，可以确保浮点运算（以及所有切断）正如早期的Java版本那样准确。切断只影响某些操作的指数。当一个类被strictfp修饰，所有的方法自动被strictfp修饰。 

strictfp的意思是FP-strict，也就是说精确浮点的意思。在Java虚拟机进行浮点运算时，如果没有指定strictfp关键字时，Java的编译器以及运行环境在对浮点运算的表达式是采取一种近似于我行我素的行为来完成这些操作，以致于得到的结果往往无法令你满意。而一旦使用了strictfp来声明一个类、接口或者方法时，那么所声明的范围内Java的编译器以及运行环境会完全依照浮点规范`IEEE-754`来执行。因此如果你想让你的浮点运算更加精确，而且不会因为不同的硬件平台所执行的结果不一致的话，那就请用关键字strictfp。
 
可以将一个类、接口以及方法声明为strictfp，但是不允许对接口中的方法以及构造函数声明strictfp关键字，例如下面的代码： 

###### 合法的使用关键字strictfp:

    strictfp interface A{}  
    public strictfp class FpDemo1{  
      strictf pvoidf(){}  
    }
     
###### 错误的使用方法:
  
    interface A {      
      strictfp void f();      
    }      
      
    public class FpDemo2 {      
      strictfp FpDemo2() {}      
    }  
 
一旦使用了关键字strictfp来声明某个类、接口或者方法时，那么在这个关键字所声明的范围内所有浮点运算都是精确的， 符合IEEE-754规范的。例如一个类被声明为strictfp，那么该类中所有的方法都是strictfp的。

##### 汇总：


关键字:transient

使用对象:字段

介绍:字段不是对象持久状态的一部分，不应该把字段和对象一起串起

--------
关键字:volatile

使用对象:字段  

介绍:写时强制刷新到共享内存，读时强制从共享内存读取。因为异步线程可以访问字段，所以有些优化操作是一定不能作用在字段上的。volatile有时可以代替synchronized

--------
关键字:strictfp

使用对象:类/接口/方法

介绍:声明的范围内Java的编译器以及运行环境会完全依照浮点规范`IEEE-754`来执行


