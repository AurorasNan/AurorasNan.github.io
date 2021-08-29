---
title: java8特性——Stream
date: 2021-08-13 10:56:04
tags: java8特性
categories: java
---
## 前言
- 说明  
	- Stream是一个来自数据源的元素队列并支持聚合操作
		- 数据源：流的来源。可以是集合、数组、I/O channel、generator等
		- 聚合操作：类似SQL语句的操作，如filter、map、find等 
	- 与Collection操作相比的特性
		- Pipelinling：中间的操作都会返回流对象本身，多个操作可以串联成一个管道
		- 内部迭代：之前的集合遍历都是通过Iterator或for each的方式，显示的在集合外部迭代，Stream提供内部迭代，通过访问者模式（Visitor）实现
<!--more-->

## 生成流
java8集合接口生成流的方式：

- stream() 创建串行流
- parallelStream() 创建并行流

```
List<String> strings = Arrays.asList("abc", "", "bc", "efg", "abcd","", "jkl");
List<String> filtered = strings.stream().filter(string -> !string.isEmpty()).collect(Collectors.toList());
```

## 常见用法
- forEach：迭代数据

```
Random random = new Random();
random.ints().limit(10).forEach(System.out::println);
```
- map：映射每个元素到对应的结果

```
List<Integer> numbers = Arrays.asList(3, 2, 2, 3, 7, 3, 5);
// 获取对应的平方数
List<Integer> squaresList = numbers.stream().map( i -> i*i).distinct().collect(Collectors.toList());
```
- filter：通过设置的条件过滤出元素

```
List<String>strings = Arrays.asList("abc", "", "bc", "efg", "abcd","", "jkl");
// 获取空字符串的数量
long count = strings.stream().filter(string -> string.isEmpty()).count();
```
- limit：获取指定数量的流

```
Random random = new Random();
random.ints().limit(10).forEach(System.out::println);
```
- sorted：对流排序

```
Random random = new Random();
random.ints().limit(10).sorted().forEach(System.out::println);
```
- Collectors：实现归约操作（将流转换成集合、聚合元素等）

```
List<String>strings = Arrays.asList("abc", "", "bc", "efg", "abcd","", "jkl");
List<String> filtered = strings.stream().filter(string -> !string.isEmpty()).collect(Collectors.toList());
 
System.out.println("筛选列表: " + filtered);
String mergedString = strings.stream().filter(string -> !string.isEmpty()).collect(Collectors.joining(", "));
System.out.println("合并字符串: " + mergedString);
```
 - Collectors.toMap()的三个重载方法
 	- Collectors.toMap(Function<? super T, ? extends K> keyMapper, Function<? super T, ? extends U> valueMapper);
 	- Collectors.toMap(Function<? super T, ? extends K> keyMapper, Function<? super T, ? extends U> valueMapper, BinaryOperator<U> mergeFunction);
 	- Collectors.toMap(Function<? super T, ? extends K> keyMapper, Function<? super T, ? extends U> valueMapper, BinaryOperator<U> mergeFunction, Suplier<M> mapSupplier);
 - 参数含义
 	- keyMapper：key的映射函数
 	- valueMapper：value的映射函数
 	- mergeFunction：当key冲突时，调用的合并方法
 	- mapSuppiler：Map构造器，在需要返回特定的Map时使用
