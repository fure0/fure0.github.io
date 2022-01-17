---
layout: post
categories: "Java"
title: "Int And Integer"
author: "TY_K"
---

## int와Integer의차이

### int (Primitive자료형 / 오브젝트가 아님)

* "자료형"을의미함 (int, float, long, double와 같은 하나의 primitive자료형을 의미함)
* "산술연산"이 가능함
* null로 초기화 할수없음 (0으로 초기화 가능) 이러한 점으로 Java/C/C++이 차이가 있음

### Integer (Wrapper클래스 / 오브젝트)

* 랩퍼 클래스
* Unboxing을 하지않으면(int로 변경하느것) 산술연산이 불가능하지만, null값은 처리가능(그렇지만 실행하면 연산이 가능. 이것은 Java에서 Auto Boxing을 해서)
* null값 처리가 용이하여 SQL과 연동할경우 처리가 용이
* 직접적인 산술연산은 불가능
* int를 클래스로 감싼 형태

## Wrapper 클래스란

primitive type（int, long, float, double ...）을 오브젝트화한 형태가 wrapper class
```java
Integer a = new Integer(3);
        
System.out.println(a.MIN_VALUE); //-2147483648
System.out.println(a.MAX_VALUE); //2147483647
```
## Auto Boxing / UnBoxing

Java에서는 자동변환 해준다
```java
int i = 1;
Integer integer = i; // int -> Integer(Auto Boxing)
int i = integer; // Integer -> int(Auto UnBoxing)
```