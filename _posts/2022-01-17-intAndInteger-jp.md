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

## Java 1.4 이하에서 Int 변환으로 정수

Java 1.4 이하 버전을 사용하는 경우 암시적 변환이 지원되지 않으므로 Integer 클래스의 intValue() 메서드를 사용하여 Integer 개체를 int 유형으로 변환. 이 메서드는 인수를 받지 않지만 기본 값을 반환한다

```java
public class SimpleTesting{
	public static void main(String[] args){
		Integer a = new Integer(10);
		System.out.println("Integer value = "+a);
		int b = a.intValue();
		System.out.println("int value = "+b);
	}
}

// Integer value = 10
// int value = 10
```

## Java에서 parseInt() 메서드를 사용하여 정수에서 Int로 변환

parseInt()는 정수 값을 int로 변환할 수 있는 Integer 메서드입니다. 문자열 인수를 가져오고 int 값을 반환합니다. 문자열 정수 객체만 있는 경우에 유용합니다. 아래의 예를 참조하십시오.

```java
public class SimpleTesting{  
	public static void main(String[] args){
		Integer a = new Integer("10");
		System.out.println("Integer value = "+a);
		int b = Integer.parseInt(a.toString());
		System.out.println("int value = "+b);
	}
}
```