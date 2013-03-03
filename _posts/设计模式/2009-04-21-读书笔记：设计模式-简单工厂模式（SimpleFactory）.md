---
layout: post
title: "读书笔记：设计模式-简单工厂模式（SimpleFactory）"
description: ""
category: 设计模式
subhead: ''
tags: [java,  设计模式,  简单工厂]

---
不想做过多的理论说明，举个例子吧。
有个鞋厂，生产耐克，李宁的鞋子，用代码实现，怎么做呢？
 

    package org.gunct.pattern;  
  
    public class ShoesFactory {  
        public void getNikeShoes() {  
            System.out.println(" 工厂生产了耐克鞋！ ");  
        }  
        public void getLiNingShoes() {  
            System.out.println(" 工厂生产了李宁鞋！ ");  
        }  
    }  
 
根据用户需求，生产不同的鞋子：
 
    public class Consumer {  
        public static void main(String[] args) {  
            ShoesFactoryfactory = new ShoesFactory();  
            if (" 用户要耐克鞋 ") {  
                factory.getNikeShoes();  
            }  
            if (" 用户要林宁鞋子 ") {  
                factory.getLiNingShoes();  
            }  
        }  
    }  

{% include JB/setup %}