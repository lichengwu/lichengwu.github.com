---
layout: post
title: "新解：给定包含4 300 000 000个32位整数的顺序文件，如何找出一个至少出现两次的整数。"
description: ""
category: algorithm
subhead: ''
tags: [java, algorithm]

---

看了编程珠玑的这个问题，一看到是顺序已经排好，很多人会想到二分查找，
其实不然，线性搜索就可以做到。

    /**
     * 问题描述： 给定包含4 300 000 000个32位整数的顺序文件， 如何找出一个至少出现两次的整数
     * 
     * @author Oliver
     * 
     */
    public class FindTwice {

        /**
         * 由于4 300 000 000 >2^32,所以必然存在重复的整数 考虑到内存的问题，可以先读取一部分，然后查找 这里假设一次读取10个
         */
        public static void main(String[] args) {
            int[] arr = { 2, 3, 4, 5, 7, 11, 12, 12, 13, 14, 15 };
            int iCount = 0;
            int increase = arr[0];
            for (; iCount < arr.length; iCount++) {
                if (arr[iCount] > iCount + increase) {
                    increase += (arr[iCount] - iCount - increase);
                    continue;
                }
                if (arr[iCount] < iCount + increase) {
                    System.out.println("重复的数字是:" + arr[iCount]);
                    break;
                }
            }
        }
    }

 
忽然又想到这个有趣的笑话：

美国宇航局花了100万研制出了能在太空写字的钢笔，请问对此前苏联是怎么做的呢？


{% include JB/setup %}