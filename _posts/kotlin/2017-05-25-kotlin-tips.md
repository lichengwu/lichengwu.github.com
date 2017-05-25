---
layout: post
catalog: true
title: "Kotlin小技巧，秒杀Java"
description: ""
category: kotlin
subhead: ''
tags: [kotlin,tips,java]

---

呵呵，看完这个，会不会让你放弃Java

## 集合操作

### list转map(associateBy)

````
val mainOrders = orderDao!!.queryUserOrder(param)
val orderMap = mainOrders.associateBy { it.id }.toMap()
````

### map的key或者value转换

假如一个map的key是String，需要转换成Long；或者map的value是一个对象，要转成另一个对象。

按照标准Java写法，可以要new一个新的map，然后循环老的map，在kotlin中，一行代码搞定

````
val map = mutableMapOf(1 to 1, 2 to 2)
val newMap = map.mapKeys { "key_${it.key}" }.mapValues { "value_${it.value}" }
println(newMap)
//打印结果 {key_1=value_1, key_2=value_2}
````
