---
layout: post
categories: "Java"
title: "Int And Integer"
author: "TY_K"
---

## intとIntegerの差異

□ int (Primitive資料型 / オブジェクトではない)

* "資料型"を意味する。 (int、float、long、doubleのような一つのprimitive資料型を意味します。)
* "算術演算"が可能です。
* nullで初期化できません。(0で初期化できます。)このような点からJavaはC/C++と少しの差があります。

□ Integer (Wrapperクラス / オブジェクト)

* ラッパークラスです。
* Unboxing をしなければ(int に変更すること)算術演算が不可能ですが、null 値は処理できます(ところが実行すると算術になる。 これはJavaでAutoun Boxingをしたからだ)
* null値処理が容易でSQLと連動する場合の処理が容易。
* 直接的な算術演算はできません。
* intをクラスに纏った形である。

## Wrapperクラスとは

primitive type（int、long、float、double ...）をオブジェクト化した形がwrapper class
```java
Integer a = new Integer(3);
        
System.out.println(a.MIN_VALUE); //-2147483648
System.out.println(a.MAX_VALUE); //2147483647
```
## Auto Boxing / UnBoxing

Javaでは自動で変換してくれる。
```java
int i = 1;
Integer integer = i; // int -> Integer(Auto Boxing)
int i = integer; // Integer -> int(Auto UnBoxing)
```