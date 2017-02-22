---
layout: post
catalog: true
title: "Java 枚举7常见种用法"
description: ""
category: java
subhead: ''
tags: [java, enmu]

---

DK1.5引入了新的类型——枚举。在 Java 中它虽然算个“小”功能，却给我的开发带来了“大”方便。

#### 用法一：常量

在JDK1.5 之前，我们定义常量都是： public static fianl.... 。现在好了，有了枚举，可以把相关的常量分组到一个枚举类型里，而且枚举提供了比常量更多的方法。
 
    public enum Color {  
      RED, GREEN, BLANK, YELLOW  
    }  
 
#### 用法二：switch
JDK1.6之前的switch语句只支持int,char,enum类型，使用枚举，能让我们的代码可读性更强。
   
    enum Signal {  
        GREEN, YELLOW, RED  
    }  
    public class TrafficLight {  
        Signal color = Signal.RED;  
        public void change() {  
            switch (color) {  
            case RED:  
                color = Signal.GREEN;  
                break;  
            case YELLOW:  
                color = Signal.RED;  
                break;  
            case GREEN:  
                color = Signal.YELLOW;  
                break;  
            }  
        }  
    }  
 
#### 用法三：向枚举中添加新方法

如果打算自定义自己的方法，那么必须在enum实例序列的最后添加一个分号。而且 Java 要求必须先定义 enum 实例。
 
    public enum Color {  
        RED("红色", 1), GREEN("绿色", 2), BLANK("白色", 3), YELLO("黄色", 4);  
        // 成员变量  
        private String name;  
        private int index;  
        // 构造方法  
        private Color(String name, int index) {  
            this.name = name;  
            this.index = index;  
        }  
        // 普通方法  
        public static String getName(int index) {  
            for (Color c : Color.values()) {  
                if (c.getIndex() == index) {  
                    return c.name;  
                }  
            }  
            return null;  
        }  
        // get set 方法  
        public String getName() {  
            return name;  
        }  
        public void setName(String name) {  
            this.name = name;  
        }  
        public int getIndex() {  
            return index;  
        }  
        public void setIndex(int index) {  
            this.index = index;  
        }  
    }  
 
#### 用法四：覆盖枚举的方法

下面给出一个toString()方法覆盖的例子。
 
 
    public enum Color {  
        RED("红色", 1), GREEN("绿色", 2), BLANK("白色", 3), YELLO("黄色", 4);  
        // 成员变量  
        private String name;  
        private int index;  
        // 构造方法  
        private Color(String name, int index) {  
            this.name = name;  
            this.index = index;  
        }  
        //覆盖方法  
        @Override  
        public String toString() {  
            return this.index+"_"+this.name;  
        }  
    }  
 
#### 用法五：实现接口

所有的枚举都继承自`java.lang.Enum`类。由于Java 不支持多继承，所以枚举对象不能再继承其他类。
 
    public interface Behaviour {  
        void print();  
        String getInfo();  
    }  
    public enum Color implements Behaviour{  
        RED("红色", 1), GREEN("绿色", 2), BLANK("白色", 3), YELLO("黄色", 4);  
        // 成员变量  
        private String name;  
        private int index;  
        // 构造方法  
        private Color(String name, int index) {  
            this.name = name;  
            this.index = index;  
        }  
        //接口方法  
        @Override  
        public String getInfo() {  
            return this.name;  
        }  
        //接口方法  
        @Override  
        public void print() {  
            System.out.println(this.index+":"+this.name);  
        }  
    }  
 
#### 用法六：使用接口组织枚举
 
    public interface Food {  
        enum Coffee implements Food{  
            BLACK_COFFEE,DECAF_COFFEE,LATTE,CAPPUCCINO  
        }  
        enum Dessert implements Food{  
            FRUIT, CAKE, GELATO  
        }  
    }  
 
#### 用法七：关于枚举集合的使用

`java.util.EnumSet`和`java.util.EnumMap`是两个枚举集合。EnumSet保证集合中的元素不重复；EnumMap中的key是enum类型，而value则可以是任意类型。关于这个两个集合的使用就不在这里赘述，可以参考JDK文档。

关于枚举的实现细节和原理请参考：

参考资料：《ThinkingInJava》第四版

