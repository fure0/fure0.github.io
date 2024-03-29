---
layout: post
categories: "Java"
title: "JVM & Java Memory Area"
author: "TY_K"
---

<style>
    table {
        border-collapse: collapse;
    }
    table tr td{
        border:1px solid;
        padding:4px;
    }
    table th:first-of-type {
        width: 25%;
    }
</style>

## JVM(Java Virtual Machine)

자바 가상 머신으로 자바 바이트 코드를 실행할 수 있는 주체다.

CPU나 운영체제(플랫폼)의 종류와 무관하게 실행이 가능하다.

즉, 운영체제 위에서 동작하는 프로세스로 자바 코드를 컴파일해서 얻은 바이트 코드를 해당 운영체제가 이해할 수 있는 기계어로 바꿔 실행시켜주는 역할을 한다.

JVM의 구성을 살펴보면 크게 4가지(Class Loader, Execution Engine, Garbage Collector, Runtime Data Area)로 나뉜다.

![JVM](https://user-images.githubusercontent.com/20508342/86537186-17f8cd80-bf28-11ea-9c63-1ed68118bd66.png)

### 1.Class Loader

자바에서 소스를 작성하면 Person.java 처럼 .java파일이 생성된다.

.java 소스를 자바컴파일러가 컴파일하면 Person.class 같은 .class파일(바이트코드)이 생성된다.

이렇게 생성된 클래스파일들을 엮어서 JVM이 운영체제로부터 할당받은 메모리영역인 Runtime Data Area로 적재하는 역할을 Class Loader가 한다. (자바 애플리케이션이 실행중일 때 이런 작업이 수행된다.)

### 2.Execution Engine

Class Loader에 의해 메모리에 적재된 클래스(바이트 코드)들을 기계어로 변경해 명령어 단위로 실행하는 역할을 한다.

명령어를 하나 하나 실행하는 인터프리터(Interpreter)방식이 있고 JIT(Just-In-Time) 컴파일러를 이용하는 방식이 있다.

JIT 컴파일러는 적절한 시간에 전체 바이트 코드를 네이티브 코드로 변경해서 Execution Engine이 네이티브로 컴파일된 코드를 실행하는 것으로 성능을 높이는 방식이다.

### 3.Garbage Collector

Garbage Collector(GC)는 Heap 메모리 영역에 생성(적재)된 객체들 중에 참조되지 않는 객체들을 탐색 후 제거하는 역할을 한다.

GC가 역할을 하는 시간은 정확히 언제인지를 알 수 없다. (참조가 없어지자마자 해제되는 것을 보장하지 않음)

또 다른 특징은 GC가 수행되는 동안 GC를 수행하는 쓰레드가 아닌 다른 모든 쓰레드가 일시정지된다.

특히 Full GC가 일어나서 수 초간 모든 쓰레드가 정지한다면 장애로 이어지는 치명적인 문제가 생길 수 있는 것이다. (GC와 관련된 내용은 아래 Heap영역 메모리를 설명할 때 더 자세히 알아본다.)

### 4.Runtime Data Area

JVM의 메모리 영역으로 자바 애플리케이션을 실행할 때 사용되는 데이터들을 적재하는 영역이다.

이 영역은 크게 Method Area, Heap Area, Stack Area, PC Register, Native Method Stack로 나눌 수 있다.

## 자바 런타임 메모리(Runtime Data area)구조

![RuntimeDataArea](https://user-images.githubusercontent.com/20508342/86537207-38288c80-bf28-11ea-89dc-ab448202d7e4.png)

### 1.Method area (메소드 영역)

클래스 멤버 변수의 이름, 데이터 타입, 접근 제어자 정보같은 필드 정보와 메소드의 이름, 리턴 타입, 파라미터, 접근 제어자 정보같은 메소드 정보, Type정보(Interface인지 class인지), Constant Pool(상수 풀 : 문자 상수, 타입, 필드, 객체 참조가 저장됨), static 변수, final class 변수등이 생성되는 영역이다.

### 2.Heap area (힙 영역)

new 키워드로 생성된 객체와 배열이 생성되는 영역이다.

메소드 영역에 로드된 클래스만 생성이 가능하고 Garbage Collector가 참조되지 않는 메모리를 확인하고 제거하는 영역이다.

### 3.Stack area (스택 영역)

지역 변수, 파라미터, 리턴 값, 연산에 사용되는 임시 값등이 생성되는 영역이다.

int a = 10; 이라는 소스를 작성했다면 정수값이 할당될 수 있는 메모리공간을 a라고 잡아두고 그 메모리 영역에 값이 10이 들어간다. 즉, 스택에 메모리에 이름이 a라고 붙여주고 값이 10인 메모리 공간을 만든다.

클래스 Person p = new Person(); 이라는 소스를 작성했다면 Person p는 스택 영역에 생성되고 new로 생성된 Person 클래스의 인스턴스는 힙 영역에 생성된다.

그리고 스택영역에 생성된 p의 값으로 힙 영역의 주소값을 가지고 있다. 즉, 스택 영역에 생성된 p가 힙 영역에 생성된 객체를 가리키고(참조하고) 있는 것이다.

메소드를 호출할 때마다 개별적으로 스택이 생성된다.

### 4.PC Register (PC 레지스터)

Thread(쓰레드)가 생성될 때마다 생성되는 영역으로 Program Counter 즉, 현재 쓰레드가 실행되는 부분의 주소와 명령을 저장하고 있는 영역이다. (*CPU의 레지스터와 다름)

이것을 이용해서 쓰레드를 돌아가면서 수행할 수 있게 한다.

### 5.Native method stack

자바 외 언어로 작성된 네이티브 코드를 위한 메모리 영역이다.

보통 C/C++등의 코드를 수행하기 위한 스택이다. (JNI)

쓰레드가 생성되었을 때 기준으로

1,2번인 메소드 영역과 힙 영역을 모든 쓰레드가 공유하고,

3,4,5번인 스택 영역과 PC 레지스터, Native method stack은 각각의 쓰레드마다 생성되고 공유되지 않는다.

### Heap area & Garbage Collector

힙 영역은 좀 더 살펴봐야하는데 그 이유는 GC의 주요 대상이기 때문이다.

(Stack영역과 Method영역도 GC의 대상이 된다)

힙 영역은 우선 5개의 영역(eden, survivor1, survivor2, old, permanent)으로 나뉜다.

-> JDK7까지는 permanent영역이 heap에 존재했습니다. JDK8부터는 permanent 영역은 사라지고 일부가 "meta space 영역"으로 변경되었습니다.(위의 그림 JDK7 기준입니다.) meta space 영역은 Native stack 영역에 포함되었습니다.

(survivor영역의 숫자는 의미없고 두 개로 나뉜다는 것이 중요하다)

힙 영역을 굳이 5개로 나눈 이유는 효율적으로 GC가 일어나게 하기 위함이다. 자세한 것은 GC가 일어나는 프로세스를 보면서 설명한다.

GC는 Minor GC와 Major GC로 나뉜다.

### Minor GC : New 영역에서 일어나는 GC

1. 최초에 객체가 생성되면 Eden영역에 생성된다.

2. Eden영역에 객체가 가득차게 되면 첫 번째 CG가 일어난다.

3. survivor1 영역에 Eden영역의 메모리를 그대로 복사된다. 그리고 survivor1 영역을 제외한 다른 영역의 객체를 제거한다.

4. Eden영역도 가득차고 survivor1영역도 가득차게된다면, Eden영역에 생성된 객체와 survivor1영역에 생성된 객체 중에 참조되고 있는 객체가 있는지 검사한다.

5. 참조 되고있지 않은 객체는 내버려두고 참조되고 있는 객체만 survivor2영역에 복사한다.

6. survivor2영역을 제외한 다른 영역의 객체들을 제거한다.

7. 위의 과정중에 일정 횟수이상 참조되고 있는 객체들을 survivor2에서 Old영역으로 이동시킨다.

- 위 과정을 계속 반복, survivor2영역까지 꽉차기 전에 계속해서 Old로 비움

### Major GC(Full GC) : Old 영역에서 일어나는 GC

1. Old 영역에 있는 모든 객체들을 검사하며 참조되고 있는지 확인한다.

2. 참조되지 않은 객체들을 모아 한 번에 제거한다.

- Minor GC보다 시간이 훨씬 많이 걸리고 실행중에 GC를 제외한 모든 쓰레드가 중지한다.

* Major GC(Full GC)가 일어나면,

Old영역에 있는 참조가 없는 객체들을 표시하고 그 해당 객체들을 모두 제거하게 된다.

그러면서 Heap 메모리 영역에 중간중간 구멍(제거되고 빈 메모리 공간)이 생기는데 이 부분을 없애기 위해 재구성을 하게 된다. (디스크 조각모음처럼 조각난 메모리를 정리함)

따라서 메모리를 옮기고 있는데 다른 쓰레드가 메모리를 사용해버리면 안되기 때문에 모든 쓰레드가 정지하게 되는 것이다.

### JDK 8에서 Perm 영역은 왜 삭제됐을까

#### 요약

+ JDK 8부터 Permanent Heap 영역이 제거되었다.
  - 대신 Metaspace 영역이 추가되었다.
  - Perm은 JVM에 의해 크기가 강제되던 영역이다.


+ Metaspace는 Native memory 영역으로, OS가 자동으로 크기를 조절한다.
  - 옵션으로 Metaspace의 크기를 줄일 수도 있다.


+ 그 결과 기존과 비교해 큰 메모리 영역을 사용할 수 있게 되었다.
  - Perm 영역 크기로 인한 java.lang.OutOfMemoryError를 더 보기 힘들어진다.


JEP 122에서는 JRockit과 Hotspot을 통일시키기 위해 PermGen 영역을 삭제한다고 한다.

### JDK 8: PermGen 제거

**What's New in JDK 8**문서를 보면 HotSpot 항목에서 다음 문장을 찾을 수 있다.

> Removal of PermGen.

### 7과 8의 비교

"JVM Performance Optimizing 및 성능분석 사례"에서는 7과 8의 HotSpot JVM 구조를 비교하고 있다

Java7 까지의 HotSpot JVM 구조를 보자.

```terminal
<----- Java Heap ----->             <--- Native Memory --->
+------+----+----+-----+-----------+--------+--------------+
| Eden | S0 | S1 | Old | Permanent | C Heap | Thread Stack |
+------+----+----+-----+-----------+--------+--------------+
                        <--------->
                       Permanent Heap
S0: Survivor 0
S1: Survivor 1
```

그리고 Java 8 HotSpot JVM 구조를 보자.

```terminal
<----- Java Heap -----> <--------- Native Memory --------->
+------+----+----+-----+-----------+--------+--------------+
| Eden | S0 | S1 | Old | Metaspace | C Heap | Thread Stack |
+------+----+----+-----+-----------+--------+--------------+
```

7에서 Java Heap과 Perm 영역은 다음과 같은 역할을 한다.

+ Java Heap
  + PermGen에 있는 클래스의 인스턴스 저장.
  + -Xms(min), -Xmx(max)로 사이즈 조정.

+ PermGen
  + 클래스와 메소드의 메타데이터 저장.
  + 상수 풀 정보.
  + JVM, JIT 관련 데이터.
  + -XX:PermSize(min), -XX:MaxPermSize(max)로 사이즈 조정.

책에서는 Perm 영역의 역할에 대해 다음과 같이 설명한다.

> Perm 영역은 보통 Class의 Meta 정보나 Method의 Meta 정보, Static 변수와 상수 정보들이 저장되는 공간으로 흔히 메타데이터 저장 영역이라고도 한다. 이 영역은 Java 8 부터는 Native 영역으로 이동하여 Metaspace 영역으로 변경되었다. (다만, 기존 Perm 영역에 존재하던 Static Object는 Heap 영역으로 옮겨져서 GC의 대상이 최대한 될 수 있도록 하였다)

#### 변경된 사항을 표로 정리하면 다음과 같다.

||Java 7|Java 8|
|--|--|--|
|Class 메타 데이터|저장|저장|
|Method 메타 데이터|저장|저장|
|Static Object 변수, 상수|저장|Heap 영역으로 이동|
|메모리 튜닝|Heap, Perm 영역 튜닝|Heap 튜닝, Native 영역은 OS가 동적 조정|
|메모리 옵션|-XX:PermSize<br/>-XX:MaxPermSize|-XX:MetaspaceSize<br/>-XX:MaxMetaspaceSize|

### 왜 Perm이 제거됐고 Metaspace 영역이 추가된 것인가?

> 최근 Java 8에서 JVM 메모리 구조적인 개선 사항으로 Perm 영역이 Metaspace 영역으로 전환되고 기존 Perm 영역은 사라지게 되었다. Metaspace 영역은 Heap이 아닌 Native 메모리 영역으로 취급하게 된다. (Heap 영역은 JVM에 의해 관리된 영역이며, Native 메모리는 OS 레벨에서 관리하는 영역으로 구분된다) Metaspace가 Native 메모리를 이용함으로서 개발자는 영역 확보의 상한을 크게 의식할 필요가 없어지게 되었다.

즉, 각종 메타 정보를 OS가 관리하는 영역으로 옮겨 Perm 영역의 사이즈 제한을 없앤 것이라 할 수 있다.

#### 원문링크

[JVM 구조와 자바 런타임 메모리 구조 (자바 애플리케이션이 실행될 때 JVM에서 일어나는 일, 과정을 설명해줄 수 있나요?)][link1]

[JDK 8에서 Perm 영역은 왜 삭제됐을까][link2]

[link1]: https://jeong-pro.tistory.com/148 "link1"

[link2]: https://johngrib.github.io/wiki/java8-why-permgen-removed/ "link2"