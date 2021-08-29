---
title: java8特性——时间日期API
date: 2021-08-13 11:42:59
tags: java8特性
categories: java
---
Java8在 java.time 包下提供了的常用API：

- Local(本地) − 简化了日期时间的处理，没有时区的问题。
- Zoned(时区) − 通过制定的时区处理日期时间。 
<!--more--> 

### 本地化时间API

```
import java.time.LocalDate;
import java.time.LocalTime;
import java.time.LocalDateTime;
import java.time.Month;
 
public class Java8Tester {
   public static void main(String args[]){
      Java8Tester java8tester = new Java8Tester();
      java8tester.testLocalDateTime();
   }
    
   public void testLocalDateTime(){
    
      // 获取当前的日期时间
      LocalDateTime currentTime = LocalDateTime.now();
      System.out.println("当前时间: " + currentTime);//当前时间: 2016-04-15T16:55:48.668
        
      LocalDate date1 = currentTime.toLocalDate();
      System.out.println("date1: " + date1);//date1: 2016-04-15
        
      Month month = currentTime.getMonth();
      int day = currentTime.getDayOfMonth();
      int seconds = currentTime.getSecond();
        
      System.out.println("月: " + month +", 日: " + day +", 秒: " + seconds);//月: APRIL, 日: 15, 秒: 48
        
      LocalDateTime date2 = currentTime.withDayOfMonth(10).withYear(2012);
      System.out.println("date2: " + date2);//月: APRIL, 日: 15, 秒: 48
        
      // 12 december 2014
      LocalDate date3 = LocalDate.of(2014, Month.DECEMBER, 12);
      System.out.println("date3: " + date3);//date3: 2014-12-12
        
      // 22 小时 15 分钟
      LocalTime date4 = LocalTime.of(22, 15);
      System.out.println("date4: " + date4);//date4: 22:15
        
      // 解析字符串
      LocalTime date5 = LocalTime.parse("20:15:30");
      System.out.println("date5: " + date5);//date5: 20:15:30
   }
}
```
### 使用时区的日期时间API

```
import java.time.ZonedDateTime;
import java.time.ZoneId;
 
public class Java8Tester {
   public static void main(String args[]){
      Java8Tester java8tester = new Java8Tester();
      java8tester.testZonedDateTime();
   }
    
   public void testZonedDateTime(){
    
      // 获取当前时间日期
      ZonedDateTime date1 = ZonedDateTime.parse("2015-12-03T10:15:30+05:30[Asia/Shanghai]");
      System.out.println("date1: " + date1);//date1: 2015-12-03T10:15:30+08:00[Asia/Shanghai]
        
      ZoneId id = ZoneId.of("Europe/Paris");
      System.out.println("ZoneId: " + id);//ZoneId: Europe/Paris
        
      ZoneId currentZone = ZoneId.systemDefault();
      System.out.println("当期时区: " + currentZone);//当期时区: Asia/Shanghai
   }
}
```