---
layout: post
title: "翻烙饼问题"
description: ""
category: algorithm
subhead: ''
tags: [algorithm, java]

---

####1.这是一个翻烙饼的问题：

一个饭馆的服务员，在每次客户点了烙饼的时候，都会把烙饼按下面最大上面最小的顺序摆放好，由于一只手托着盘子里的烙饼，只能用另一只手一次抓住最上面的几个烙饼进行翻个儿。

求最优的翻烙饼方法（翻的次数最少）详见《程序之美》。

####2.建模：

书上给的算法没看懂，不过我觉得这个也可以实现，但肯定不是最优的。

每次翻转的时候的目标都是最大的烙饼，第一次先把最大的烙饼翻个儿到最上面，

然后再把它翻个到最下面，这样就完成了一个烙饼的翻个，接下来重复下面的N-1个烙饼，直到全部翻转完成。

    public class Flapjack {
        private static int[] arr = { 1, 4, 6, 8, 45, 2, 3, 9, 5, 6, 12, 4, 56, 7 };
        private static int revertCount;

        /**
         * isSorted 是否已排序好
         * 
         * @param arr
         * @return
         */
        private static boolean isSorted() {
            for (int i = 1; i < arr.length; i++) {
                if (arr[i - 1] > arr[i])
                    return false;
            }
            return true;
        }

        /**
         * getMaxItemIndex 获取数组中最大项的索引
         * 
         * @param end
         * @return
         */
        private static int getMaxItemIndex(int end) {
            if (end > arr.length)
                throw new ArrayIndexOutOfBoundsException();
            int temp = arr[0];
            int index = 0;
            for (int i = 1; i < end; i++) {
                if (temp < arr[i]) {
                    temp = arr[i];
                    index = i;
                }
            }
            return index;
        }

        /**
         * revert 翻转
         * 
         * @param begin
         * @param end
         */
        private static void revert(int begin, int end) {
            if (begin > end || end > arr.length - 1 || begin < 0)
                return;
            for (int iBegin = begin, iEnd = end; iBegin < iEnd; iBegin++, iEnd--) {
                int temp = arr[iBegin];
                arr[iBegin] = arr[iEnd];
                arr[iEnd] = temp;
            }
            revertCount++;
        }

        /**
         * 排序主函数
         * 
         * @param len
         */
        public static void sort(int len) {
            if (isSorted())
                return;
            int maxIndex = getMaxItemIndex(len);
            revert(0, maxIndex);
            revert(0, len - 1);
            if (len <= 0)
                return;
            sort(len - 1);
        }

        /**
         * 打印
         */
        public static void print() {
            System.out.print("[");
            for (int i = 0; i < arr.length; i++)
                if (i == arr.length - 1)
                    System.out.print(arr[i]);
                else
                    System.out.print(arr[i] + ",");
            System.out.print("]");
            System.out.println("翻转次数：" + revertCount);
        }

        /**
         * 测试
         */
        public static void main(String[] args) {
            System.out.println("排序前：");
            Flapjack.print();
            sort(arr.length);
            System.out.println("排序后：");
            Flapjack.print();
        }
    }

如有更好的算法，欢迎讨论。

{% include JB/setup %}