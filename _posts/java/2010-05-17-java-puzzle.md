---
layout: post
title: "java中的陷阱 你注意了么？"
description: ""
category: java
subhead: ''
tags: [java, puzzle]

---

总结几个经典的java陷阱给大家。

答案隐藏了，Ctrl+A显示。建议先思考一下结果，然后运行代码试验。也许你会恍然大悟。

#### 1、找奇数：
 
    public static boolean isOdd(int i){  
         return i % 2 == 1;  
    } 
 
上面的方法真的能找到所有的奇数么？

A：<span style="color:#fff;">没有考虑到负数问题，如果传参是负数，那么永远不能得到结果！应该是：return i % 2 != 0;</span>

#### 2、浮点数相减：
    System.out.println(2.0-1.9);  
A：<span style="color:#fff;">不会，精度问题。正确做法：用decimal。</span>

#### 3、交换：
    int x = 2010; 
    int y = 2012; 
    x^=y^=x^=y;
    System.out.println("x= " + x + "; y= " + y);
x、y的值互换了么？

A：<span style="color:#fff;">没有，java运算顺序是从左到右的，应该这么写：y=(x^= (y^= x))^ y;</span>

#### 4、字符和字符串：
    System.out.println("H" + "a");
    System.out.println('H' + 'a');
上面两个语句输出结果相同么？

A:<span style="color:#fff;">不相同，字符会被转换成在数字。所以第一句输出：Ha，第二句输出两个字符的assii码相加的数字。</span>

#### 5、无限循环:
    public static final int END = Integer.MAX_VALUE; 
    public static final int START = END - 100; 
    public static void main(String[] args) {
	    int count = 0; 
	    for (int i = START; i <= END; i++) 
		    count++; 
	    System.out.println(count); 
	}
上面程序运行的结果是什么？

A:<span style="color:#fff;">无限循环。将i&lt;=END改成i&lt;END？为什么呢？你知道的，呵呵！</span>

#### 6、计数器问题

    int minutes = 0;   
    for (int ms = 0; ms < 60*60*1000; ms++)   
        if (ms % 60*1000 == 0)   
            minutes++;   
    System.out.println(minutes);  
 
结果跟你想的一样么？

A:<span style="color:#fff;">括号问题，不多说！</span>

#### 7、到底返回什么？

    public static boolean decision() {   
         try {   
            return true;   
        } finally {   
          return false;   
        }   
    }   
 
true？false？

A：<span style="color:#fff;">一般情况下，不管怎么说try/catch代码块中，finally总是最后被执行的。</span>

#### 8、错误里聚集遍历

    public static void main(String[] args) {  
        Vector v = new Vector();  
        v.add("one");  
        v.add("two");  
        v.add("three");  
        v.add("four");  
        Enumeration enume = v.elements();  
        while (enume.hasMoreElements()) {  
            String s = (String) enume.nextElement();  
            if (s.equals("two"))  
                v.remove("two");  
            else {  
                System.out.println(s);  
            }  
        }  
        System.out.println("What's really there...");  
        enume = v.elements();  
        while (enume.hasMoreElements()) {  
            String s = (String) enume.nextElement();  
            System.out.println(s);  
        }  
    }  
 
运行代码看看结果跟你想的一样么？

A:<span style="color:#fff;">一般不建议在遍历聚集的时候对聚集进行操作。为什么结果是这样呢？看JDK源码能得到答案。Enumeration没有实现Fail Fast操作，如果换成ArrayList，上面的代码可能会出错。《java与模式》迭代子（iterator）介绍了。</span>

推荐：《Java解惑》



