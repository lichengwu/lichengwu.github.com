---
layout: post
title: "Eratosthenes筛选法求素数"
description: "prime Eratosthenes"
category: algorithm
subhead: ''
tags: [algorithm, java, prime]

---


###素数定义

素数，又称质数，是指在大于1的自然数中，除了1和本身外，无法被其他自然数整除的数。

###素数求解

根据素数定义，不难想到求解一个自然数i是否是素数的办法是在[2,Math.sqrt(i)]范围内的自然数内尝试整除。


    public static boolean isPrime(long num) {
        for (long i = 2; i <= Math.sqrt(num); i++) {
            if (num % i == 0) {
                return false;
            }
        }
        return true;
    }

Eratosthenes算法：在自然数中2是最小的素数，任何2的整数倍数(1倍以上)都不是素数；同理3的任何整数倍数都不是素数。想象有一把筛子，里面都是自然数。把2的倍数都筛出去，再把3，5，7...素数的倍数都筛出去，剩下的就全是素数了。具体实现方法如下：

    public static List<Integer> prime(int num) {
        boolean[] flags = new boolean[num + 1];
        //初始化，认为都是素数
        for (int i = 0; i < flags.length; i++) {
            flags[i] = true;
        }
        //筛选
        for (int i = 2; i < Math.sqrt(num); i++) {
            if (flags[i]) {
                for (int multi = 2, j = multi * i; j <= num; multi++, j = multi * i) {
                    if (flags[j] && j % i == 0) {
                        flags[j] = false;
                    }
                }
            }
        }
        //整理结果
        List<Integer> rs = new ArrayList<>();
        for (int i = 2; i < flags.length; i++) {
            if (flags[i]) {
                rs.add(i);
            }

        }
        return rs;
    }
    
###性能测试

针对上面两中算法，计算`Short.MAX_VALUE * 500`内的所有素数：


    @Test
    public void test() {
        //第一种方式
        long start = System.currentTimeMillis();
        List<Integer> prime1 = new ArrayList<>();
        int num = Short.MAX_VALUE * 500;
        for (int i = 2; i < num; i++) {
            if (isPrime(i)) {
                prime1.add(i);
            }
        }
        System.out.println("prime1 time used:" + (System.currentTimeMillis() - start) + "ms");
        start = System.currentTimeMillis();

        //第二种方式
        List<Integer> prime2 = prime(num);
        System.out.println("prime2 time used:" + (System.currentTimeMillis() - start) + "ms");

        //数据校验
        Assert.assertTrue(Objects.deepEquals(prime1, prime2));
    }
    
测试结果：

    prime1 time used:38321ms
    prime2 time used:322ms   
       
将近120倍的时间差距，而且随着数字范围扩大，差距会变得更大。


