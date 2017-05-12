---
layout: post
catalog: true
title: "Kotlin问题汇总"
description: ""
category: kotlin
subhead: ''
tags: [kotlin,reference]

---

### 与Spring AOP结合

在`默认`情况下，kotlin定义的非抽象class是final类型，字段也是一样；而在Spring中，AOP是不能代理final的类或者成员变量的，
final的类代理会报异常，而final的成员变量代理后是`null`。目前这是个很隐蔽的坑，有的时候在运行时才能发现。解决方法就是在类或者成员变量上加上`open`修饰。

````
@Service
open class OrderPersistFun {
    @Resource
    open var orderPayAssembleFun: OrderPayAssembleFun? = null
    @Resource
    open var orderManager: OrderManager? = null
}

````

### 与Java混编

在IntelliJ IDEA中安装kotlin插件后，对于kotlin项目idea会自动提示配合kotlin，通常会自动把pom文件加入kotlin的依赖和编译插件。
当项目有java和kotlin同时存在的时候，maven编译会报错：在Java文件中找不到kotlin的类或者包。解决方法是加入maven compiler插件。

````
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-compiler-plugin</artifactId>
    <version>3.3</version>
    <executions>
        <execution>
            <id>default-compile</id>
            <phase>none</phase>
        </execution>
        <execution>
            <id>default-testCompile</id>
            <phase>none</phase>
        </execution>
        <execution>
            <id>compile</id>
            <phase>compile</phase>
            <goals>
                <goal>compile</goal>
            </goals>
        </execution>
        <execution>
            <id>testCompile</id>
            <phase>test-compile</phase>
            <goals>
                <goal>testCompile</goal>
            </goals>
        </execution>
    </executions>
    <configuration>
        <target>${maven.compiler.target}</target>
        <source>${maven.compiler.source}</source>
        <encoding>${project.build.sourceEncoding}</encoding>
    </configuration>
</plugin>

````
