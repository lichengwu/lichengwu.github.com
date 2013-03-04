---
layout: post
title: "算法：插入排序"
description: ""
category: algorithm
subhead: ''
tags: [algorithm,  java]

---

最近想认认真真，仔仔细细学习一下算法。
网上刚刚买了《算法导论》，今天看了一点，做个记录。
代码给上！
 

    package oliver.algorithm.sort;
    
    public class InsertionSort {
    
        public static void sort(int [] arr)
        {
            int temp;
            for(int j=1;j<arr.length;j++)
            {
                temp=arr[j];
                int i=j-1;
                while(i>=0&&arr[i]>temp)
                {
                    arr[i+1]=arr[i];
                    i--;
                }
                arr[i+1]=temp;
            }
        }
    } 

  
 
测试代码：


    package oliver.algorithm.sort;
    
    public class InsertionSortTest {
    
        /**
         * @param args
         */
        public static void main(String[] args) {
            // TODO Auto-generated method stub  
            int [] arr={3,6,1,0,5,7,4,9,3};
            InsertionSort.sort(arr);
            for(int a:arr)
            {
                System.out.println(a+" ");
            }
        }
    
    }  
 


{% include JB/setup %}
