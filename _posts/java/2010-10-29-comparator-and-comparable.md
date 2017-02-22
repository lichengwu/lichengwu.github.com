---
layout: post
catalog: true
title: "集合排序 Comparator Comparable"
description: ""
category: java
subhead: ''
tags: [java, sort, collections, comparable, comparator]

---

复习，顺便记录下来！
通过Collections.sort 和  Arrays.sort 对对象排序时，有两种方式，排序对象实现Comparable接口重写compareTo方法和给sort方法传递实现Comparator接口的参数，下面的SortObject对象，实现了这两中方法。

    public class HashSetTest {
        // 自定义MyString类
        static class MyString {
            String value;

            public MyString(String value) {
                this.value = value;
            }
        }

        public static void main(String[] args) {
            // 创建一个HashSet对象
            Set<Object> set = new HashSet<Object>();
            // 创建连个String对象
            String s1 = new String("a");
            String s2 = new String("a");
            // 创建连个MyString对象
            MyString s3 = new MyString("a");
            MyString s4 = new MyString("a");
            // 添加元素
            set.add(s1);
            set.add(s2);
            set.add(s3);
            set.add(s4);
            // 看看对象的equals
            System.out.println("s1.equals(s2):" + s1.equals(s2));
            System.out.println("s3.equals(s4):" + s3.equals(s4));
            // 打印几个大小及里面的元素
            System.out.println("set size:" + set.size());
            for (Iterator<Object> it = set.iterator(); it.hasNext();) {
                System.out.println(it.next());
            }
        }
    }
   
  
 
测试代码:
 
    public class TestMain {
        @SuppressWarnings("unchecked")
        public static void main(String[] args) {
            List sortList = new ArrayList(Arrays.asList(new SortObject("Jhon"), new SortObject(
                    "Oliver"), new SortObject("David"), new SortObject("Oliver"), new SortObject(
                    "Lucy"), new SortObject("Mark"), new SortObject("Oliver")));
            System.out.println("before sort:" + sortList.toString());
            // 下面这两句效果相同

            // Collections.sort(sortList);
            Collections.sort(sortList, new SortObject.SortObjectComparator());
            System.out.println("after sort:" + sortList.toString());
        }
    }  

