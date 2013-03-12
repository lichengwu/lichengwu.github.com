---
layout: post
title: "由象棋中的“将”和“帅”位置引出的一个算法"
description: ""
category: algorithm
subhead: ''
tags: [algorithm, java]

---

#### 1.问题描述：
假设中国象棋的棋盘上只有“将”和“帅”，这两个棋子。

根据象棋的规则，写出“将”和“帅”所有可能的合法位置。

要求只能声明一个变量。
 
#### 2.建模
由棋盘上的布局可知，“将”和“帅”的运动范围在一个3×3
的格子里。

    1——2——3
    |  |  |
    4——5——6
    |  |  |
    7——8——9
上面的模型模拟“将”或“帅”的所有可能位置，因为“将”和“帅”。

不能同时在同一列，所以当“将”在1,4,7的位置时，“帅”在2,3,5,6,8,9位置。反之亦然。

下面用A代表“将”，用B代表“帅”来进行模拟。
 
 
    public class Chess {

        /**
         * 第一种方法
         */
        public static void solutionA() {
            // 包括不符合规则的情况，一共有81中情况
            int iCount = 81;
            while (--iCount > 0)
                // iCount/9%3表示列
                // iCount%9%3表示行
                if (iCount / 9 % 3 != iCount % 9 % 3)
                    // 输出符合规则的情况
                    System.out.println("A=" + (iCount / 9 + 1) + " B=" + (iCount % 9 + 1));
        }

        /**
         * 第二种方法
         */
        public static void solutionB() {
            // 定义一个类作为变量，C中可以用结构体表示
            class Position {
                // A的位置
                public int A;
                // B的位置
                public int B;
            }
            // 声明一个变量
            Position p = new Position();
            for (p.A = 1; p.A <= 9; p.A++)
                for (p.B = 1; p.B <= 9; p.B++)
                    if (p.A % 3 != p.B % 3)
                        System.out.println("A=" + p.A + " B=" + p.B);
        }

        /**
         * 测试
         */
        public static void main(String[] args) {
            // 测试方法A
            System.out.println("======第一种方法======");
            Chess.solutionA();
            // 测试方法B
            System.out.println("======第二种方法======");
            Chess.solutionB();
        }
    }
 
这个问题及算法转自《编程之美》。


{% include JB/setup %}