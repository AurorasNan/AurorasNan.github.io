---
title: java8特性——方法引用
date: 2021-08-12 15:14:07
tags: java8特性
categories: java
---
## 前言
- 说明  
通过方法的名字指向一个方法
- 作用  
代码 -> 紧凑简洁、减少冗余 

## 语法  
### 类::静态方法
<!--more-->
```
@FunctionalInterface
public interface ImTheOne {
    String handleString(String a, String b);
}
class OneClass {
    public static String concatString(String a, String b) {
        return a + b;
    }
}
 
public class Test {
    public static void main(String[] args) {
        ImTheOne theOne = OneClass::concatString;
        String result = theOne.handleString("abc", "def");
        System.out.println(result);
 
        //相当于以下效果，直接把类的静态方法写在Lambda体里
        ImTheOne theOne2 = (a, b) -> OneClass.concatString(a, b);
        String result2 = theOne2.handleString("123", "456");
        System.out.println(result2);
 
    }
}
```
### 对象::实例方法
```
@FunctionalInterface
public interface ImTheOne {
    String handleString(String a, String b);
}
class OneClass {
    public String concatString(String a, String b) {
        return a + b;
    }
}
 
public class Test {
    public static void main(String[] args) {
        OneClass oneClass = new OneClass();
        ImTheOne theOne = oneClass::concatString;
        String result = theOne.handleString("abc", "def");
        System.out.println(result);
 
        //相当于以下效果
        OneClass oneClass2 = new OneClass();
        ImTheOne theOne2 = (a, b) -> oneClass2.concatString(a, b);
        String result2 = theOne2.handleString("123", "456");
        System.out.println(result2);
 
    }
}
```
### 类::实例方法
这种模式实际上是对象::实例方法模式的一种变形，当一个对象调用方法时，方法的某个参数是函数式接口，而且函数式接口的方法参数列表的第一个参数就是调用者对象所属的类时，可以引用调用者类中定义的，不包含函数式接口第一个参数的方法，并用类::实例方法这种形式来表达，比如这样： 

``` java
@FunctionalInterface
public interface ImTheOne<T> {
    String handleString(T a, String b);
}
class OneClass {
    String oneString;
 
    public String concatString(String a) {
        return this.oneString + a;
    }
 
    public String startHandleString(ImTheOne<OneClass> imTheOne, String str) {
        String result = imTheOne.handleString(this, str);
        return result;
    }
}

public class Test {
    public static void main(String[] args) {
        OneClass oneClass = new OneClass();
        oneClass.oneString = "abc";
        String result = oneClass.startHandleString(OneClass::concatString, "123");
        System.out.println(result);//输出结果：abc123456
        //相当于以下效果
        OneClass oneClass2 = new OneClass();
        oneClass2.oneString = "abc";
        ImTheOne theOne2 = (a, b) -> oneClass2.concatString(b);
        String result2 = theOne2.handleString(theOne2, "123");
        System.out.println(result2);//输出结果：abc123456

 
    }
}
```
代码解释如下：  

|步骤| 描述 | 代码对照 |  
|:-:|:---|:-------|
|1  |当一个对象调用一个方法|oneClass对象，调用startHandleString(ImTheOne<OneClass> imTheOne, String str)方法。|
|2|	方法的参数中包含一个函数式接口|	startHandleString方法中有个参数是ImTheOne<OneClass>，ImTheOne<T>是一个函数式接口，在本例中使用的泛型是OneClass类。|
|3	|该函数式接口的第一个参数类型是这个对象的类|ImTheOne<T>这个函数式接口的唯一抽象方法是handleString(T t, String a, String b)，第一个参数就是ImTheOne定义时的泛型类，也就是OneClass类，也就是步骤1时调用者oneClass对象所属的类。|
|4|	那么这个函数式接口可用方法引用代替|	对于ImTheOne<OneClass>这个参数，可以用OneClass::concatString这个方法引用来代替。|
|5|	并且替换用的方法可以不包含函数式接口的第一个参数|函数式接口中的handleString方法参数列表是 (T a, String b)，2个参数，而方法引用中的concatString方法参数是(String a)，比handleString少了第一个T参数，此时java会认为第一个参数就是方法的调用者oneClass对象。|

<font color='red'>注：</font>这种模式下方法引用的方法必须是调用者对象所属类中的对象，也就是concatString方法必须定义在OneClass类中。
### Class::new
- 这种模式被称为构造方法引用，或构造器引用。
- 构造方法引用实际上表示一个函数式接口中的唯一方法引用了一个类的构造方法，引用的是那个参数相同的构造方法。

```
@FunctionalInterface
public interface ImTheOne {
    TargetClass getTargetClass(String a);
}
class TargetClass {
    String oneString;
 
    public TargetClass() {
        oneString = "default";
    }
 
    public TargetClass(String a) {
        oneString = a;
    }
}
 
public class Test {
    public static void main(String[] args) {
 
        ImTheOne imTheOne = TargetClass::new;
        TargetClass targetClass = imTheOne.getTargetClass("abc");
        System.out.println(targetClass.oneString);//abc
 
        //相当于以下效果
        ImTheOne imTheOne2 = (a) -> new TargetClass("abc");
        TargetClass targetClass2 = imTheOne2.getTargetClass("123");
        System.out.println(targetClass2.oneString);//abc
    }
}
```
<font color='red'>注：</font>

1.函数式接口的方法getTargetClass(String a)有一个String类型的参数，所以当使用构造方法引用时，引用的是有一个String参数的那个构造，也就是这个：

```
public TargetClass(String a) {
    oneString = a;
}
```
2.第二行输出的不是123，而是abc，因为Lambda表达式的原因，当我们执行：```imTheOne2.getTargetClass("123");```这行代码时，调用的实际上是Lambda体：```new TargetClass("abc");```所以输出的结果和字符串"123"没关系。

### 数组::new
数组引用算是构造器引用的一种，可以引用一个数组的构造

```
@FunctionalInterface
public interface ImTheOne<T> {
    T getArr(int a);
}
public class Test {
    public static void main(String[] args) {
        ImTheOne<int[]> imTheOne = int[]::new;
        int[] stringArr = imTheOne.getArr(5);
        System.out.println(stringArr.length);
    }
}
```
<font color='red'>注：</font>
使用数组引用时，函数式接口中抽象方法必须是有参数的，而且参数只能有一个，必须是数字类型int或Integer，这个参数代表的是将来生成数组的长度。

 

