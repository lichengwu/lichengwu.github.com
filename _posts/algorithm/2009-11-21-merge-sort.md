---
layout: post
title: "算法：归并排序"
description: ""
category: algorithm
subhead: ''
tags: [sort,  algorithm,  java,  merge-sort]

---

又花了点时间看看算法，这次看的是归并。
感觉归并的效率比插入排序高多了！
代码给上:
 

    package oliver.algorithm.sort;
    public class MergeSort {
        public static void sort(int[] arr,int first,int last)
        {
            int mid=0;
            if(first<last)
            {
                mid=(first+last)/2;
                sort(arr,first,mid);
                sort(arr,mid+1,last);
                merge(arr,first,mid,last);
            }
        }
        private static void merge(int [] arr,int p,int q,int r)
        {
            int begin1,end1,begin2,end2;
            begin1=p;end1=q;
            begin2=q+1;end2=r;
            int []temp=new int[r-p+1];
            int i=0;
            while((begin1<=end1)&&(begin2<=end2))
            {
                if(arr[begin1]<arr[begin2])
                {
                    temp[i]=arr[begin1];
                    begin1++;
                }
                else
                {
                    temp[i]=arr[begin2];
                    begin2++;
                }
                i++;
            }
            while(begin1<=end1)
            {
                temp[i++]=arr[begin1++];
            }
            while(begin2<=end2)
            {
                temp[i++]=arr[begin2++];
            }
            for(i=0;i<=(r-p);i++)
            {
                arr[p+i]=temp[i];
            }
        }
    }  
  
 
测试代码:
    
    package oliver.algorithm.sort;
    import java.util.Date;
    public class MergeSortTest {
        /**
         * @param args
         */
        public static void main(String[] args) {
            int size=100000;
            int [] arr=new int[size];
            for(int i=0;i<size;i++)
            {
                arr[i]=size-i;
            }
            Long beginTime=new Date().getTime();
            MergeSort.sort(arr,0,size-1);
            Long endTime=new Date().getTime();
            for(int a:arr)
            {
                System.out.print(a+" ");
            }
            System.out.println();
            System.out.println("when n = "+size+" toke "+(endTime-beginTime)+"ms");
        }
    }  
 

{% include JB/setup %}
