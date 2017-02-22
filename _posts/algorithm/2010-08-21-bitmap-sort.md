---
layout: post
catalog: true
title: "基于位图、位向量的快速排序"
description: ""
category: algorithm
subhead: ''
tags: [algorithm, bitmap, vector-diagram]

---

#### 需求：

##### 1.需要就N位数字进行排序(N>5) 

##### 2.N位数是一个稠密的数字集合

##### 3.集合中没有重复的元素(数字)

#### 限制：

##### 1.尽量减少内存使用

##### 2.要求算法时间要短
 
先写一个工具类，生成稠密集合:

    public class InitArray  
    {  
        public static Integer[] getArray(int nums)  
        {  
            int count=0;  
            Random random = new Random(new Date().getTime());  
            Set<Integer> set = new HashSet<Integer>();  
            for(;count<nums;count++)  
            {  
                set.add((int)(random.nextFloat()*nums));  
            }  
            return set.toArray(new Integer[]{});  
        }  
    }  
 
排序实现:

    public class BitMapSort  
    {  
        //N 数字数量级  
        public static int NUMS=1000;  
        /** 
        * Logger for this class 
        */  
        private static final Log logger = LogFactory.getLog(BitMapSort.class);  
        @SuppressWarnings("deprecation")  
        public static Integer[] sort(Integer array[])  
        {  
          
            byte[] loop =new byte[NUMS];  
          
            //置0 java默认为0  
    //      for(int i=0;i<NUMS;i++)  
    //      {  
    //          loop[i]=0;  
    //      }  
            logger.info("--------------开始位向量排序--------------");  
            Date begin = new Date();  
            //将数据插入位图  
            for(int j=0;j<array.length;j++)  
            {  
                loop[array[j]]=1;  
            }  
            //输出排序  
            for(int j=0,k=0;k<NUMS;k++)  
            {  
                if(loop[k]==1)  
                {  
                    array[j++]=k;  
                }  
            }  
            Date end = new Date();  
            logger.info("--------------位向量排序结束--------------");  
            logger.info("--------------位向量排序耗时:"+(end.getMinutes()-begin.getMinutes())+"分钟 "+(end.getSeconds()-begin.getSeconds())+"秒--------------");  
            return array;  
        }  
    }  
 
#### 测试：

 
    public class Main  
    {  
        /** 
        * Logger for this class 
        */  
        private static final Log logger = LogFactory.getLog(Main.class);  
      
        private static int NUMS=BitMapSort.NUMS;  
        /** 
         * <b>main。</b>   
         * <p><b>详细说明：</b></p> 
         * <!-- 在此添加详细说明 --> 
         * 无。 
         * @param args 
         */  
        public static void main(String[] args)  
        {  
            logger.info("--------------生存"+NUMS+"以内的稠密数组--------------");  
            Integer array[] = InitArray.getArray(NUMS);  
            logger.info("--------------生成完毕--------------");  
            Integer arr1[] = BitMapSort.sort(array);  
        }  
    }  
 
 
说明：这个排序的速度很快，但是适合稠密的数据集合，不然会浪费很多内存。

