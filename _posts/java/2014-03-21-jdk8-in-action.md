---
layout: post
title: "JDK8实战"
description: "Jdk8新特性介绍"
category: java
subhead: ''
tags: [java, jdk,  jdk8]
---

# 方法扩展
jdk8为接口(Interface)提供了默认方法支持，使用`default`关键字，就可以声明默认方法。所有实现类都可以访问默认方法，而且也可以重写默认方法。
另外，jdk8接口也支持默认静态方法，为我们写工具类提供一种选择(以前写工具类都是声明一个final的类，并且声明默认构造函数为private)。

    public interface MyInterface {
        void normalMethod();
        default void defaultMethod() {
            System.out.println("defaultMethod");
        }
        static void utilMethod() {
            System.out.println("utilMethod");
        }
    }

    public class MethodExtension implements MyInterface {
        @Override
        public void normalMethod() {
            System.out.println("normalMethod");
        } 
    }

    MethodExtension extension = new MethodExtension();
    extension.normalMethod();//normalMethod
    extension.defaultMethod();//normalMethod
    MyInterface.utilMethod();//utilMethod


# Lambda表达式
lambda表达式为java提供闭包功能，替换原来单一的抽象方法。

lambda表达式的底层实现就是上面介绍的`方法扩展`。每个lambda表达式都是一个绑定特定类型的默认方法。

下面从一个简单的例子来展示一下lambda的用法：

有一个简单的JavaBean，有两个成员变量，name和age。我们下面的例子也是基于这个对象。

    public class Person {
        public Person() {}
        public Person(String name, Integer age) {
            this.name = name;
            this.age = age;
        }
        private String name;
        private Integer age;
        public String getName() {
            return name;
        }
        public void setName(String name) {
            this.name = name;
        }
        public Integer getAge() {
            return age;
        }
        public void setAge(Integer age) {
            this.age = age;
        }
        public String toString() {
            return "Person{name=" + name + "age=" + age + "}";
        }
    }


有一个Person需要根据name排序：


	List<Person> personList = Arrays.asList(new Person("name3", 18), new Person("name1", 24), new Person("name2", 20));

在jdk8之前，我们一般这么写：

    personList.sort(new Comparator<Person>() {
         @Override
         public int compare(Person a, Person b) {
              return a.getName().compareTo(b.getName());
         }
    });

在这里，我们创建了一个匿名类，实现了compare方法。

再来看看lambda写法：
```
personList.sort((a, b) -> a.getName().compareTo(b.getName()));
```
如此简洁，省略了匿名类，return和方法声明的`{}`，一行代码搞定。(a, b)表示一个需要两个参数的方法声明，这里没有指定参数类型，lamdba能自动判断。`->`后面是方法体。

## Functional Interfaces
Jdk8中lambda是如何实现的？每个lambda表达式都会匹配一个特殊类型的接口(Interface)，在Jdk8规范中，这个特殊的接口叫`Functional Interface`。这个接口需要声明一个`@FunctionalInterface`注解。`Functional Interface`只能有一个抽象方法，但是可以有多个默认方法。如果声明多个抽象方法，编译器会报错。

下面是具体例子：

    @FunctionalInterface
    public interface ToStringFunction<T> {
        String convert2String(T t);
        default String origin2String(T t) {
            return t.toString();
        }
    }

    ToStringFunction<Person> ps = (person) -> person.getName() + ":" + person.getAge();
    Person person = new Person("oliver", 25);
    System.out.println(ps.origin2String(person));//Person{name=oliverage=25}
    System.out.println(ps.convert2String(person));//oliver:25



## 绑定方法和构造函数引用
`Functional Interface`中的抽象方法除了自己实现外，还可以指向方法和构造函数。方法引用通过关键字`::`来实现。需要注意的是，被绑定的方法参数要和`Functional Interface`中的抽象方法参数列表匹配。

### 绑定静态方法：
通过`类名::静态方法名`能访问类的静态方法。

    ToStringFunction<Person> ps = String::valueOf; //static method


### 构造函数：
构造函数通过`类名::new`来访问。

    @FunctionalInterface
    public interface PersonFactory {
        Person create(String name,Integer age);
    }

    PersonFactory factory = Person::new; //constructor
    Person person = factory.create("oliver", 25);
    System.out.println(person);

### 绑定实例方法：
实例方法和静态方法一样，通过`类名::方法名`来访问。下面的java.util.function.ToIntFunction是jdk8内置Functional Interface。

    ToIntFunction<Person> ps = Person::getAge;//instance method
    Person pp = new Person("oliver", 25);
    System.out.println(ps.applyAsInt(pp));25


## Lambda作用域

在Lambda中如何访问表达式外部的变量？

访问本地变量：与匿名类不同，lambda访问本地变量不要求本定变量一定声明`final`：

    int df = 20;//declare as final is also ok
    ToStringFunction<Person> ps = (person) -> person.getAge() + person.getName() + df;

如果编译器发现本地变量在lambda作用域之外被修改了或者在lambda内被修改了，这样的修改是不被允许的：

    int df = 20;
    ToStringFunction<Person> ps = (person) -> person.getAge() + person.getName() + df;
    df = 21;


    int df = 20;
    ToStringFunction<Person> ps = (person) -> {
        String s = person.getAge() + person.getName() + df;
        df = 21;
        return s;
    };

     
上面这两段代码编译失败：

> local variables referenced from a lambda expression must be final or effectively final

访问静态变量：静态变量的读写在lamdba表达式作用域内外都可以。

    static int df = 20;
    @Test
    public void testScope() {
        ToStringFunction<Person> ps = (person) -> {
            String s = person.getAge() + person.getName() + df;
            df = 21;//inner
            return s;
        };
        df = 22;//outer
    }


## 内建Functional Interfaces
Jdk8提供了丰富的内建Functional Interfaces，放在`java.util.function`包下面，下面举几个常用的类：

### Predicate
Predicate根据一个特定类型参数判断返回一个bool，一般用于逻辑计算。

    Predicate<String> hasLength = (str) -> str.length() > 0;
    Predicate<String> isNull = (str) -> str == null;
    Predicate<String> isEmpty = isNull.negate().and(hasLength);
    System.out.println(isEmpty.test("")); //false
    System.out.println(isEmpty.test(null)); //false

### Function
Function接收一个参数并返回一个结果，主要用于把多个方法链接起来：

    Function<String, String> s1 = String::valueOf;
    Function<String, String> s2 = String::valueOf;
    Function<String, String> all = s1.andThen(s2);

### Supplier
Supplier根据给定类型，返回一个实例。不接受任何参数：

    Supplier<Person> supplier = Person::new;
    Person person = supplier.get();

### Consumer
Consumer有点像Function，但是Consumer只有输入参数，没有返回值。

    Consumer<Person> print = System.out::println;
    print.accept(person);

### Optional
Optional不是Functional Interfaces，而是一个容器工具，用来避免`NullPointerException`。一般用于方法调用的返回结果，把结果内容放在这个容器内。`isPresent()`方法用来测试容器中的内容是否为`null`。

    Optional<Integer> opt1 = Optional.of(10);
    System.out.println(opt1.isPresent()); //true
    System.out.println(opt1.get()); //10
    System.out.println(opt1.filter((i) -> i > 11).isPresent()); //false
    System.out.println(opt1.filter((i) -> i > 11).get()); //java.util.NoSuchElementException: No value present

# 链式API
Stream采用链式对集合进行操作，包括过滤，排序，迭代，去重等。

    List<Integer> list = Arrays.asList(77, 1, 4, 5, 2, 77, 22);
    list.stream().distinct().filter((s) -> s > 10).sorted().forEach(System.out::println);

### 并发链式API
并发Stream采用多线程方式对集合进行操作，包括过滤，排序，迭代，去重等。

    public void testParallelStream() {
        int size = 1000000;
        List<Integer> list = new ArrayList<Integer>(size);
        for (int i = 0; i < size; i++) {
            list.add((int) (Math.random() * size));
        }
        long being = System.currentTimeMillis();
        long count1 = list.stream().filter((i) -> Math.sin(i) > 0).sorted().count();
        System.out.println("stream time used:" + (System.currentTimeMillis() - being));
        being = System.currentTimeMillis();
        long count2 = list.parallelStream().filter((i) -> Math.sin(i) > 0).sorted().count();
        Assert.assertEquals(count1, count2);
        System.out.println("parallelStream time used:" + (System.currentTimeMillis() - being));
    }

    //output
    stream time used:626
    parallelStream time used:295


# Date-Time API
Jdk提供增强的日期和时间API，放在`java.time`包下。`java.time`基于国际便准(IOS)的日历系统，支持全球日历。

## Clock
`Clock`用于方法日期和时间，并且它跟时区相关。跟`System.currentTimeMillis()`一样，可以访问当前时间的毫秒。`Clock`中用瞬间`Instant`这里类来记录时间轴上的一个点，可以用这个类来记录应用事件。`Instant`可以用来创建`java.util.Date`对象。

    Clock shanghai = Clock.systemDefaultZone();
    Clock tokyo = Clock.system(ZoneId.of("Asia/Tokyo"));
    System.out.println(shanghai.millis());//1395408039822
    System.out.println(tokyo.millis());//1395408039822
    System.out.println(tokyo.getZone());//Asia/Tokyo
    System.out.println(shanghai.getZone());//Asia/Shanghai
    Instant instant = shanghai.instant();
    Date date = Date.from(instant);//Fri Mar 21 21:20:39 CST 2014
    System.out.println(date);


## Timezones
时区在新API中用`ZoneId`表示。时区中定义的`偏移`能够用来转换时间日期。


    ZoneId shanghai = ZoneId.systemDefault();
    ZoneId tokyo = ZoneId.of("Asia/Tokyo");
    System.out.println(shanghai.getRules()); //ZoneRules[currentStandardOffset=+08:00]
    System.out.println(tokyo.getRules()); //ZoneRules[currentStandardOffset=+09:00]
    Clock myClock = Clock.system(shanghai);
    System.out.println(Date.from(myClock.instant())); //Fri Mar 21 21:30:07 CST 2014
    System.out.println(myClock.instant().atZone(tokyo)); //2014-03-21T22:30:07.085+09:00[Asia/Tokyo]

## LocalTime

`LocalTime`，顾名思义，用就记录本地时间，不带时区并且不可改变(immutable)。如：22:38:39.961


    LocalTime shanghai = LocalTime.now();
    LocalTime tokyo = LocalTime.now(ZoneId.of("Asia/Tokyo"));
    System.out.println(shanghai); //21:38:39.960
    System.out.println(tokyo);  //22:38:39.961
    System.out.println(shanghai.isBefore(tokyo));   //true
    System.out.println(ChronoUnit.HOURS.between(shanghai, tokyo));   // 1
    System.out.println(ChronoUnit.MINUTES.between(shanghai, tokyo)); // 60


## LocalDate
`LocalDate`与`LocalTime`类似，用于记录不带时区的日期。下面展示如何创建和使用LocalDate的常用API。

    LocalDate now = LocalDate.now();
    LocalDate tomorrow = now.plus(1, ChronoUnit.DAYS);
    System.out.println(tomorrow); //2014-03-22
    LocalDate yesterday = now.minus(1, ChronoUnit.DAYS);
    System.out.println(yesterday); //2014-03-20
    System.out.println(now.getYear()); ///2014
    System.out.println(now.getMonth()); //MARCH
    System.out.println(now.getDayOfWeek()); //FRIDAY

## LocalDateTime
把`LocalDate`与`LocalTime`结合起来，就是LocalDateTime了，这里不再赘述，展示一下简单用法。

    LocalDateTime now = LocalDateTime.now();
    System.out.println(now); //2014-03-21T22:00:46.894
    LocalDateTime fest = LocalDateTime.of(2014, Month.DECEMBER, 1, 23, 59, 59);
    System.out.println(fest); //2014-12-01T23:59:59
    System.out.println(fest.isAfter(fest)); //false

## DateTimeFormatter

新的时间日期API提供了新的格式化工具`DateTimeFormatter`，对比以前的`java.text.DateFormat`，新工具的一大亮点是线程安全。用法类似：

    DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
    LocalDate fest = LocalDate.parse("2014-10-01 23:59:59", formatter);
    System.out.println(fest);



