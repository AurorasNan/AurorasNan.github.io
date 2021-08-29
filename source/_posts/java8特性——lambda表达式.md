---
title: java8特性——lambda表达式
date: 2021-08-11 22:00:27
tags: java8特性
categories: java
---
## 前言
- 说明  
lambda表达式可以将函数作为方法的参数，即函数作为参数传递进方法中
- 作用  
代码 -> 简洁、紧凑 

## 语法  
### lambda表达式语法格式  
```(parameters) -> expression``` 或 ```(parameters) -> {statements;} ```  
<!--more--> 
### lambda表达式特征  
- 可选类型声明：不需要声明参数类型，编译器可以统一识别参数值
- 可选的参数圆括号：一个参数无需定义圆括号，但多个参数需要定义圆括号
- 可选的大括号：如果主题函数包含了一个语句，就不需要大括号
- 可选的返回关键字：如果主体只有一个表达式返回值则编译器回自动返回值，大括号需要指明表达式返回了一个数值 
  
### 变量作用域  
- lambda表达式只能引用标记了final的外层局部变量，即<font color ='red'>不能在lambda内部修改定义在域外的局部变量</font>     
 
``` java
public class Java8Tester {
    public static void main(String args[]) {
        final int num = 1;
        Converter<Integer, String> s = (param) -> System.out.println(String.valueOf(param + num));
        s.convert(2);  // 输出结果为 3
        num=5;//报错信息：Local variable num defined in an enclosing scope must be final or effectively 
    }
 
    public interface Converter<T1, T2> {
        void convert(int i);
    }
}
```
- 在lambda表达式当中不允许声明一个与局部变量同名的参数或者局部变量
 
```
String first = "";  
Comparator<String> comparator = (first, second) -> Integer.compare(first.length(), second.length());  //编译会出错 
```