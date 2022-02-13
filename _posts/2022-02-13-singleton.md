---
layout: post
categories: "DesignPattern"
title: "Singleton"
author: "TY_K"
---

### [생성 패턴] 싱글톤(Singleton) 패턴을 구현하는 6가지 방법

첫 번째로 소개할 디자인 패턴은 **싱글톤(Singleton)** 패턴입니다.

종종 싱글톤 패턴을 '단일체' 패턴으로 번역하고 있는 책도 있지만, 일반적으로 싱글톤 패턴이라고 부릅니다.

싱글톤 패턴은 객체지향 디자인 패턴에서 가장 유명한 패턴 중 하나로, 디자인 패턴을 따로 공부하지 않으신 분들도 익히 알고 있는 패턴입니다.

하지만 유명한 만큼 예제 코드를 쉽게 접할 수 있어서인지 프로젝트에 싱글톤을 어설프게 적용은 하지만 정작 왜 써야 하는지, 어떻게 써야 내 상황에 맞게 잘 쓰는지에 대해서는 잘 모르시는 분들이 많습니다.

이번 포스팅에서는 디자인 패턴 관점에서 싱글톤의 개념과 역할에 대해 살펴보고, Java를 통해 간단한 구현까지도 실습해보도록 하겠습니다.

### 싱글톤은 생성 패턴(Creational Pattern) 중 하나이다.

생성 패턴은 인스턴스를 만드는 절차를 추상화하는 패턴입니다.

생성 패턴에 속하는 패턴들은 객체를 생성, 합성하는 방법이나 객체의 표현 방법을 시스템과 분리해줍니다.

생성 패턴은 시스템이 상속(inheritance) 보다 복합(composite) 방법을 사용하는 방향으로 진화되어 가면서 더 중요해지고 있습니다.

생성 패턴에서는 중요한 이슈가 두 가지 있습니다.

1. 생성 패턴은 시스템이 어떤 Concrete Class를 사용하는지에 대한 정보를 캡슐화합니다.
2. 생성 패턴은 이들 클래스의 인스턴스들이 어떻게 만들고 어떻게 결합하는지에 대한 부분을 완전히 가려줍니다.

쉬운 말로 정리 하자면, 생성 패턴을 이용하면 무엇이 생성되고, 누가 이것을 생성하며, 이것이 어떻게 생성되는지, 언제 생성할 것인지 결정하는 데 유연성을 확보할 수 있게 됩니다.

### 싱글톤(Singleton) 패턴이란?

싱글톤 패턴은 어떤 클래스의 인스턴스가 오직 하나임을 보장하며, 이 인스턴스에 접근할 수 있는 전역적인 접촉점을 제공하는 패턴입니다.

정리하자면, 프로그램 시작부터 종료 시까지 어떤 클래스의 인스턴스가 메모리 상에 단 하나만 존재할 수 있게 하고 이 인스턴스에 대해 어디에서나 접근할 수 있도록 하는 패턴입니다.

#### 싱글톤 패턴은 왜 고안되었을까요?

개발을 하다보면 어떤 클래스에 대해 단 하나의 인스턴스만을 갖도록 하는 것이 좋은 경우가 있습니다.

예를 들어, 로그를 찍는(Logging) 객체라던가 쓰레드 풀, 윈도우 관리자 등 여러 객체를 관리하는 역할의 객체는 프로그램 내에서 단 하나의 인스턴스를 갖는 것이 바람직합니다.

#### 그러면 그 하나의 인스턴스를 어떻게 접근할 수 있을까요?

흔히 어디에서나 접근할 수 있다고 한다면 "전역 변수"를 떠올리기가 쉽습니다. 물론 "틀렸다"라고는 할 수 없겠으나 이보다 더 좋은 방법은 클래스 자신이 자기의 유일한 인스턴스로 접근하는 방법을 자체적으로 관리하는 것입니다.

쉽게 말해, 생성자를 private하게 만들어 클래스 외부에서는 인스턴스를 생성하지 못하게 차단하고, 내부에서 단 하나의 인스턴스를 생성하여 외부에는 그 인스턴스에 대한 접근 방법을 제공할 수 있습니다.

### 싱글톤(Singleton) 구현 방법

싱글톤 패턴을 구현하는 방법은 굉장히 다양합니다.

그러나 각각의 패턴이 공통적으로 갖는 특징이 있는데, 이는 다음과 같습니다.

* private 생성자만을 정의해 외부 클래스로부터 인스턴스 생성을 차단합니다.
* 싱글톤을 구현하고자 하는 클래스 내부에 멤버 변수로써 private static 객체 변수를 만듭니다.
* public static 메소드를 통해 외부에서 싱글톤 인스턴스에 접근할 수 있도록 접점을 제공합니다.

#### 1. Eager Initialization

Eager Initialization은 가장 간단한 형태의 구현 방법입니다. 이는 싱글톤 클래스의 인스턴스를 클래스 로딩 단계에서 생성하는 방법입니다.

그러나 어플리케이션에서 해당 인스턴스를 사용하지 않더라도 인스턴스를 생성하기 때문에 자칫 낭비가 발생할 수 있습니다.

예제 코드는 아래와 같습니다.

```java
public class Singleton {
    
    private static final Singleton instance = new Singleton();
    
    // private constructor to avoid client applications to use constructor
    private Singleton(){}
 
    public static Singleton getInstance(){
        return instance;
    }
}
```

이 방법을 사용할 때는 싱글톤 클래스가 다소 적은 리소스를 다룰 때여야 합니다.

File System, Database Connection 등 큰 리소스들을 다루는 싱글톤을 구현할 때는 위와 같은 방식보다는 getInstance() 메소드가 호출될 때까지 싱글톤 인스턴스를 생성하지 않는 것이 더 좋습니다.

게다가 Eager Initializaion은 Exception에 대한 Handling도 제공하지 않습니다.

#### 2. Static Block Initialization

Static Block Initialization은 1번에서 살펴본 Eager Initialization과 유사하지만 static block을 통해서 Exception Handling에 대한 옵션을 제공합니다.

```java
public class Singleton {
 
    private static Singleton instance;
    
    private Singleton(){}
    
    //static block initialization for exception handling
    static{
        try{
            instance = new Singleton();
        }catch(Exception e){
            throw new RuntimeException("Exception occured in creating singleton instance");
        }
    }
    
    public static Singleton getInstance(){
        return instance;
    }
}
```

위와 같이 구현할 경우 싱글톤 클래스의 인스턴스를 생성할 때 발생할 수 있는 예외에 대한 처리를 할 수 있지만, Eager Initialization과 마찬가지로 클래스 로딩 단계에서 인스턴스를 생성하기 때문에 여전히 큰 리소스를 다루는 경우에는 적합하지 않게 됩니다.

#### 3. Lazy Initialization

Lazy Initialization은 이름에 걸맞게, 앞선 두 방식과는 달리 나중에 초기화하는 방법입니다.

이는 global access 한 getInstance() 메소드를 호출할 때에 인스턴스가 없다면 생성합니다.

```java
public class Singleton {
 
    private static Singleton instance;
    
    private Singleton(){}
    
    public static Singleton getInstance(){
        if(instance == null){
            instance = new Singleton();
        }
        return instance;
    }
}
```

이 방식으로 구현할 경우 1, 2번에서 안고 있던 문제(사용하지 않았을 경우에는 인스턴스가 낭비)에 대해 어느 정도 해결책이 됩니다. 그러나 이 경우에도 문제점이 있습니다. 그건 바로 **multi-thread 환경에서 동기화 문제**입니다.

만약 인스턴스가 생성되지 않은 시점에서 여러 쓰레드가 동시에 getInstance()를 호출한다면 예상치 못한 결과를 얻을 수 있을뿐더러, 단 하나의 인스턴스를 생성한다는 싱글톤 패턴에 위반하는 문제점이 야기될 수 있습니다.

그렇기에 이 방법으로 구현을 해도 괜찮은 경우는 single-thread 환경이 보장됐을 때입니다.

#### 4. Thread Safe Singleton

Thread Safe Singleton은 3번의 문제를 해결하기 위한 방법으로, getInstance() 메소드에 synchronized를 걸어두는 방식입니다. synchronized 키워드는 임계 영역(Critical Section)을 형성해 해당 영역에 오직 하나의 쓰레드만 접근 가능하게 해 줍니다. 코드는 아래와 같습니다.

```java
public class Singleton {
 
    private static Singleton instance;
    
    private Singleton(){}
    
    public static synchronized Singleton getInstance(){
        if(instance == null){
            instance = new Singleton();
        }
        return instance;
    }
    
}
```

위와 같은 방식으로 구현한다면 getInstance() 메소드 내에 진입하는 쓰레드가 하나로 보장받기 때문에 멀티 쓰레드 환경에서도 정상 동작하게 됩니다. 그러나 synchronized 키워드 자체에 대한 비용이 크기 때문에 싱글톤 인스턴스 호출이 잦은 **어플리케이션에서는 성능이 떨어지게 됩니다.**

그래서 고안된 방식이 double checked locking 입니다.

이는 getInstance() 메소드 수준에 lock을 걸지 않고 instance가 null일 경우에만 synchronized가 동작하도록 합니다.

코드는 아래와 같습니다.

```java
public static Singleton getInstance(){
    if(instance == null){
        synchronized (Singleton.class) {
            if(instance == null){
                instance = new Singleton();
            }
        }
    }
    return instance;
}
```

#### 5. Bill Pugh Singleton Implementaion

이는 Bill Pugh가 고안한 방식으로, inner static helper class를 사용하는 방식입니다.

앞선 방식이 안고 있는 문제점들을 대부분 해결한 방식으로, 현재 가장 널리 쓰이는 싱글톤 구현 방법입니다.

코드를 먼저 살펴보고 이어서 설명하겠습니다.

```java
public class Singleton {
 
    private Singleton(){}
    
    private static class SingletonHelper{
        private static final Singleton INSTANCE = new Singleton();
    }
    
    public static Singleton getInstance(){
        return SingletonHelper.INSTANCE;
    }
}
```

inner class로 인해 복잡해 보일 수 있지만 생각보다 간단합니다.

private inner static class를 두어 싱글톤 인스턴스를 갖게 합니다.

이때 1번이나 2번 방식과의 차이점이라면 SingletonHelper 클래스는 Singleton 클래스가 Load 될 때에도 Load 되지 않다가 getInstance()가 호출됐을 때 비로소 JVM 메모리에 로드되고, 인스턴스를 생성하게 됩니다.

아울러 synchronized를 사용하지 않기 때문에 4번에서 문제가 되었던 성능 저하 또한 해결됩니다.

#### 6. Enum Singleton

앞서 1~5번에서 살펴본 싱글톤 방식은 사실 완전히 안전할 수 없습니다. 왜냐하면 Java의 Reflection을 통해서 싱글톤을 파괴할 수 있기 때문입니다.

이에 Java 계의 거장 Joshua Bloch는 Enum으로 싱글톤을 구현하는 방법을 제안했습니다.

```java
public enum EnumSingleton {
 
    INSTANCE;
    
    public static void doSomething(){
        //do something
    }
}
```

그러나 이 방법 또한 1, 2번과 같이 사용하지 않았을 경우의 메모리 문제를 해결하지 못한 것과 유연성이 떨어진다는 면에서의 한계를 지니고 있습니다.

이렇게 싱글톤에 대해 자세히 알아보고, 다양한 구현 방법을 살펴보았습니다.

각각의 방식마다 장단점이 있어 무엇이 항상 옳은 구현이라고 할 수 없지만 5번에서 살펴본 inner static class 방식을 사용하는 것이 대부분의 경우에 최선의 방법이 아닐까 생각됩니다.

실제로 유명한 자바 오픈 소스 프로젝트의 코드를 살펴보시면 5번의 방식으로 싱글톤을 사용하고 있는 것을 확인할 수 있습니다.

#### 원문링크

[[생성 패턴] 싱글톤(Singleton) 패턴을 구현하는 6가지 방법][link1]

[link1]: https://readystory.tistory.com/116?category=822867 "link1"

