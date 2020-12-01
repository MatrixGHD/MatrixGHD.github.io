---
layout:     post
title:      HelloJava
subtitle:   Java简介
date:       2020-11-14
author:     matrix
header-img: img/code.jpeg
catalog: true
tags:
    - javacode
---
## Java的发展史
JAVA源于于90年代的sun公司Green项目，为了满足消费电子产品嵌入式处理器芯片代码跨平台运行的需求，解决C++过于复杂的问题，JAVA给予C++改造而来，去除了 C++复杂的指针和内存管理，并结合嵌入式系统的实时性要求，那时还叫Oak，因此Java一开始是为硬件而生。
到了1995年，他们用 OaK 语言研发了一种能将小程序嵌入到网页中执行的技术——Applet，Oak正式更名为Java，Java也开始应用于互联网。
1996年jdk1.0正式发布。
## Java的一些概念
![-c520](/img/16053380189903.jpg)
### Java体系
#### JavaSE
标准版:各应用平台的基础，桌面开发和低端商务应用的解决方案。
![-c719](/img/16053382249139.jpg)

#### JavaEE
企业版:以企业为环境而开发应用程序的解决方案
#### JavaME
微型版:致力于消费产品和嵌入式设备的解决方案
### Java技术的两种核心机制
#### Java 虚拟机(Java Virtual Machine)JVM
Java源代码通过编译器被编译成字节码，和类库一起输入到类装载器中进行字节码的验证，再输入到JVM中实现与操作系统的通信，最后操作系统执行相应的操作。
![-c700](/img/16053380930003.jpg)
![-c780](/img/16053379702720.jpg)
JVM 可以理解成一个可运行 Java 字节码的虚拟计算机系统
作用：实现代码跨平台运行
- 它有一个解释器组件，可以实现Java字节码和计算机操作系统之间的通信
- 不同操作系统有不同的JVM
#### 垃圾回收器(Garbage Collection)GC
在 C/C++等语言中，由程序员负责回收无用内存。
而Java的JVM 提供了一种系统线程跟踪存储空间的分配情况。并在 JVM 的空闲时，检查并释放那些可以被释放的存储空间。 垃圾回收器在 Java 程序运行过程中自动启用，程序员无法精确控制和干预。
### Java 开发工具集(Java Development Kits)JDK
JDK包含JRE及java编译器、Java运行时解释器、Java文档优化工具等工具和资源，如下图所示
![-c719](/img/16053382249139.jpg)

#### JRE(JavaRuntimeEnvironment)Java运行时环境
- 加载代码:由类加载器(classloader)完成;
- 校验代码:由字节码校验器(bytecodeverifier)完成;
- 执行代码:由运行时解释器(runtimeinterpreter)完成。

##### JVM
已在Java的核心机制中做了介绍
##### JavaAPI
#### Java 编译器(javac.exe)、Java 运行时解释器(java.exe)、Java 文档化化工具(javadoc.exe)及其它工具及资源
## 环境搭建
**MACOS搭建方法**
### 1、前往oracle官网下载jdk11和jdk8,以便之后可以切换环境
### 2、添加环境变量
```
export JAVA_8_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_261.jdk/Contents/Home
export JAVA_11_HOME=/Library/Java/JavaVirtualMachines/jdk-11.0.8.jdk/Contents/Home
export JAVA_HOME=$JAVA_8_HOME
```
![-c938](/img/16053219569433.jpg)
### 3、用alias设置jdk8及jdk11别名，实现不同jdk切换
```
alias jdk8='export JAVA_HOME=$JAVA_8_HOME'
alias jdk11='export JAVA_HOME=$JAVA_11_HOME'
```
![-c492](/img/16053211083332.jpg)
![-c797](/img/16053221864945.jpg)
### 4、安装eclipse
Eclipse 是一个开放源代码的、基于 Java 的可扩展开发平台。就其本身而言，它只是一个框架和一组服务， 用于通过插件组件构建开发环境。幸运的是，Eclipse 附带了一个标准的插件集，包括 Java 开发工具(Java Development Kit，JDK)。
#### 前往eclipse.org下载相应安装包并安装
#### 在eclipse中创建Java工程
设置保存路径
![-c605](/img/16053226598362.jpg)
创建Java工程
![-w544](/img/16053228098435.jpg)
填写工程名，完成
![-c817](/img/16053229759660.jpg)
不创建模块信息文件
![-c593](/img/16053230396913.jpg)
至此，工程创建完成。
## Hello world

```java
/*
多行注释
class 必须编写在.java 文件中
语法规则:
java 是严格区分大小写的
java 是一种自由格式的语言 代码分为结构定义语句和功能执行语句
功能执行语句的最后必须用分号结束，所以，你应该知道哪些是结构定义语句了
*/
package com.runoob.com;
/**
这是文档注释，方便在程序调用时查看注释信息
*/
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello World");
    }
}
```
![-c1528](/img/16053397388250.jpg)


