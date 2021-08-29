---
title: java8特性——函数式接口
date: 2021-08-13 09:59:16
tags: java8特性
categories: java
---
## 前言
- 说明  
1.有且仅有一个抽象方法，但可以有个多非抽象方法的借口  
2.可隐式转换成lambda表达式

## java.util.function常用函数式接口

|序号|接口|描述|
|:---:|:---|:----|
|1|BiConsumer\<T,U>|代表一个接受两个输入参数的操作，并且不返回任何结果|
|2|BiFunction\<T,U,R>|代表一个接受两个输入参数的操作，并且返回一个结果|
|3|BinaryOperator\<T>|代表一个作用于两个同类型操作符的操作，，并且返回一个结果|
|4|BiPredicate\<T,U>|代表一个接受两个输入参数的操作，并且返回boolean结果|
|5|Consumer\<T>|接受一个输入参数的无返回操作|
|6|Function\<T,R>|接受一个输入参数，返回一个结果|
|7|Suppiler\<T>|无参数，返回一个结果|
## Function\<T,R> 

- apply(T t) 
<!--more-->

```
Function<String, String> function = a -> a + " Jack!";
System.out.println(function.apply("Hello")); // Hello Jack!
```
- andThen(Function<? super R,? extends V> after)

```
Function<String, String> function = a -> a + " Jack!";
Function<String, String> function1 = a -> a + " Bob!";
String greet = function.andThen(function1).apply("Hello");
System.out.println(greet); // Hello Jack! Bob!
```
- cpmpose(Function<? super V,? extends T> before)

``` java
Function<String, String> function = a -> a + " Jack!";
Function<String, String> function1 = a -> a + " Bob!";
String greet = function.compose(function1).apply("Hello");
System.out.println(greet); // Hello Bob! Jack!
```
## Supplier\<T>
- 说明：生产数据函数式接口 
- 目的：生产数据 

```
Supplier<String> supplier = () -> "Hello Jack!";
System.out.println(supplier.get()); // Hello Jack!
```