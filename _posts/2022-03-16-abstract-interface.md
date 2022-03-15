---
layout: post
categories: "Java"
title: "abstract class and interface"
author: "TY_K"
---

### 추상클래스와 인터페이스 차이

#### 추상클래스 (abstract class)

- 정의 : 한 개 이상의 추상메서드를 가지는 클래스

- 목적 : 추상메서드는 선언만되며 구현이 되지 않은 불완전한 메서드이므로 객체로 생성되지 않는다.

- 일반클래스 : 세부적이고, 구체적
  - 예시 : 고양이, 사자, 강아지
- 추상클래스 : 추상적
  - 예시 : 고양이과, 개과 

#### 추상메서드

- 정의 : 내용이 없는 메서드, 구현(정의)는 하지 않고 **선언만 한 메서드**

- 목적 : 메서드의 내용이 너무 일반적인 내용이므로 부모 클래스에서 구체화하여 정의할 필요가 없을 경우, 추상메서드로 선언만하고 **상속받은 자식 클래스에서 재정의**하도록 할 때 사용

**추상 클래스는 반드시 하나 이상의 추상메서드**를 가지며, **객체를 생성할 수 없다.**

하지만 **슈퍼클래스로 사용**할 수 있으며 추상메서드를 사용하기 위해 **반드시 해당 메서드를 재정의** 해야만한다.

위 말을 코드로 보면 이해가 쉽다.

< 추상클래스 형식 >

```java
// 추상클래스 
public abstract class AbstractTest { 
    // 추상메서드 
    public abstract void dogName(String name); 
    // 일반메서드 
    public void dog(){ 
        System.out.println("개 이름 알아보기"); 
    } 
}
```

① 하나 이상의 추상메서드를 가진다 → 추상메서드 : dogName

② 객체를 생성할 수 없다 → 다음과 같이 new 추상클래스(); 로 생성할 수 없다.

```java
public class MainTest { 
  public static void main(String[] args){ 
    // 객체로 생성할 수 없다 (X) 
    AbstractTest abt = new AbstractTest(); 
  } 
}
```

③ 슈퍼클래스로 사용할 수 있고, 추상메서드는 반드시 재정의해야한다 

```java
// 추상클래스 
public class Test1 extends AbstractTest { 
  // 추상메서드 재정의 
  @Override public void dogName(String name){ 
    System.out.println("개 이름은 " + name +"입니다."); 
  } 
}
```

하여 사용방법은 상속받은 자식클래스 객체를 생성한다

```java
public class MainTest { 
  public static void main(String[] args){ 
  // 상속받은 자식클래스로 객체를 생성한다 (O) 
    AbstractTest abt = new Test1("깐돌이"); 
  } 
}
```

#### 인터페이스

추상메서드와 상수로만 이루어져 있다. 즉, 로직을 작성할 수 없다.

< 인터페이스 형식 >

```java
// 인터페이스 형식 
public interface DogTest { 
  void LargeDog(); 
  void SmallDog(); 
}
```

- 다중 상속 가능

```java
// 인터페이스 다중상속 가능 
public interface 인터페이스명 extends 상속받을 인터페이스명1, 상속받을 인터페이스명2, ...{ 
  void 추상메서드1(); ... 
}
```

< 클래스가 인터페이스 참조 형식 >

```java
public class DogTestImpl implements DogTest { 
  @Override void LargeDog(){ 
    System.out.pringln("리트리버, 보더콜리"); 
  } 
  @Override void SmallDog(){ 
    System.out.println("시츄, 말티즈"); 
  } 
}
```

#### 추상클래스와 인터페이스의 공통점 및 차이점

![스크린샷 2022-03-16 오전 12 21 51](https://user-images.githubusercontent.com/20508342/158411559-ea9bc04d-f641-456f-a800-b954958d92ae.png)

#### 원문링크

[[Java] 추상클래스와 인터페이스 차이][link1]

[link1]: https://haenny.tistory.com/162 "link1"