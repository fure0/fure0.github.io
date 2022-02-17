---
layout: post
categories: "DesignPattern"
title: "Adapter pattern"
author: "TY_K"
---

### [구조 패턴] 어댑터 패턴(Adapter Pattern) 이해

**어댑터 패턴은 구조 패턴(Structural Pattern) 중 하나로, 서로 관계없는 인터페이스들을 함께 사용할 수 있게 하는 패턴입니다.**

### 구조 패턴(Structural Pattern)이란?

구조 패턴이란 작은 클래스들을 상속과 합성을 이용하여 더 큰 클래스를 생성하는 방법을 제공하는 패턴입니다.

이 패턴을 사용하면 서로 독립적으로 개발한 클래스 라이브러리를 마치 하나인 양 사용할 수 있습니다. 또, 여러 인터페이스를 합성(Composite)하여 서로 다른 인터페이스들의 통일된 추상을 제공합니다.

 

구조 패턴의 중요한 포인트는 인터페이스나 구현을 복합하는 것이 아니라 객체를 합성하는 방법을 제공한다는 것입니다. 이는 **컴파일 단계에서가 아닌 런타임 단계에서 복합 방법이나 대상을 변경할 수 있다는 점에서 유연성을 갖습니다.**

### 어댑터 패턴(Adapter Pattern) 개념 및 예제 코드

어댑터가 일상생활에서 쓰이는 용도를 생각해보시면 인풋과 아웃풋이 다른, 이를테면 해외여행 갈 때 챙기는 볼트 변환기(돼지코) 같은 것들을 어댑터라고 표현하기도 합니다.

마찬가지로 **어댑터 패턴은 클래스의 인터페이스를 사용자가 기대하는 인터페이스 형태로 변환시키는 패턴입니다.**

어댑터 패턴은 서로 일치하지 않는 인터페이스를 갖는 클래스들을 함께 동작시킵니다.

위에서 말씀드린 것처럼 볼트 변환기 개념을 가지고서 예제코드를 작성해보도록 하겠습니다.

Volt 클래스는 단순히 volt 값을 가지고 있는 POJO 클래스입니다.

```java
public class Volt {
 
    private int volts;
	
    public Volt(int v){
        this.volts=v;
    }
 
    public int getVolts() {
        return volts;
    }
 
    public void setVolts(int volts) {
        this.volts = volts;
    }
	
}
```

Socket 클래스는 단순히 120 볼트를 값으로 갖는 볼트 객체를 생성하는 클래스입니다. 

```java
public class Socket {
 
    public Volt getVolt(){
        return new Volt(120);
    }
}
```

이제 우리는 120볼트뿐만 아니라 3볼트와 12볼트도 추가로 생성하는 어댑터를 만들어볼 것인데, 이를 위해 각각의 볼트 객체 생성을 위한 인터페이스를 정의해주겠습니다.

```java
public interface SocketAdapter {
 
    public Volt get120Volt();
		
    public Volt get12Volt();
	
    public Volt get3Volt();
}
```

어댑터 패턴을 구현하기 위해서는 Class Adapter와 Object Adapter 두 가지 방법이 있습니다.

어떤 방법으로 구현하던 결과는 같습니다.

* Class Adapter - 자바의 상속(Inheritance)을 이용한 방법입니다.
* Object Adapter - 자바의 합성(Composite)을 이용한 방법입니다.

**이제 우리는 Socket 클래스와 Volt 클래스를 SocketAdapter 인터페이스에서 정의한 메소드에 맞춰서 사용할 것입니다.**

그러면 Class Adpater와 Object Adapter 방식으로 각각 예제코드를 작성해보겠습니다.

#### Class Adapter 방식

```java
//Using inheritance for adapter pattern
public class SocketClassAdapterImpl extends Socket implements SocketAdapter{
 
    @Override
    public Volt get120Volt() {
        return getVolt();
    }
 
    @Override
    public Volt get12Volt() {
        Volt v= getVolt();
        return convertVolt(v,10);
    }
 
    @Override
    public Volt get3Volt() {
        Volt v= getVolt();
        return convertVolt(v,40);
    }
	
    private Volt convertVolt(Volt v, int i) {
        return new Volt(v.getVolts()/i);
    }
 
}
```

#### Object Adapter 방식

```java
public class SocketObjectAdapterImpl implements SocketAdapter{
 
    //Using Composition for adapter pattern
    private Socket sock = new Socket();
	
    @Override
    public Volt get120Volt() {
        return sock.getVolt();
    }
 
    @Override
    public Volt get12Volt() {
        Volt v= sock.getVolt();
        return convertVolt(v,10);
    }
 
    @Override
    public Volt get3Volt() {
        Volt v= sock.getVolt();
        return convertVolt(v,40);
    }
	
    private Volt convertVolt(Volt v, int i) {
        return new Volt(v.getVolts()/i);
    }
}
```

눈여겨 볼 것은 두 가지 방식 모두 SocketAdapter 인터페이스를 구현했다는 것입니다. 이 어댑터 인터페이스는 추상 클래스로 사용해도 무방합니다.

이제 이 어댑터 인터페이스를 사용 하여 테스트 코드를 작성해보겠습니다.

```java
public class AdapterPatternTest {
 
    public static void main(String[] args) {
		
        testClassAdapter();
        testObjectAdapter();
    }
 
    private static void testObjectAdapter() {
        SocketAdapter sockAdapter = new SocketObjectAdapterImpl();
        Volt v3 = getVolt(sockAdapter,3);
        Volt v12 = getVolt(sockAdapter,12);
        Volt v120 = getVolt(sockAdapter,120);
        System.out.println("v3 volts using Object Adapter="+v3.getVolts());
        System.out.println("v12 volts using Object Adapter="+v12.getVolts());
        System.out.println("v120 volts using Object Adapter="+v120.getVolts());
    }
 
    private static void testClassAdapter() {
        SocketAdapter sockAdapter = new SocketClassAdapterImpl();
        Volt v3 = getVolt(sockAdapter,3);
        Volt v12 = getVolt(sockAdapter,12);
        Volt v120 = getVolt(sockAdapter,120);
        System.out.println("v3 volts using Class Adapter="+v3.getVolts());
        System.out.println("v12 volts using Class Adapter="+v12.getVolts());
        System.out.println("v120 volts using Class Adapter="+v120.getVolts());
    }
	
    private static Volt getVolt(SocketAdapter sockAdapter, int i) {
        switch (i){
        case 3: return sockAdapter.get3Volt();
        case 12: return sockAdapter.get12Volt();
        case 120: return sockAdapter.get120Volt();
        default: return sockAdapter.get120Volt();
        }
    }
}
```

```java
v3 volts using Class Adapter=3
v12 volts using Class Adapter=12
v120 volts using Class Adapter=120
v3 volts using Object Adapter=3
v12 volts using Object Adapter=12
v120 volts using Object Adapter=120
```

이렇게 해서 어댑터 패턴에 대해 살펴봤습니다.

이 어댑터 패턴은 Java의 JDK 안에서 Arrays.asList()나 InputStreamReader(InputStream), OutputStreamWriter(OutputStream) 등에서 사용되었습니다. 

#### 원문링크

[[구조 패턴] 어댑터 패턴(Adapter Pattern) 이해 및 예제][link1]

[link1]: https://readystory.tistory.com/125?category=822867 "link1"