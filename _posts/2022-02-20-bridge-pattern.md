---
layout: post
categories: "DesignPattern"
title: "Bridge pattern"
author: "TY_K"
---

### [구조 패턴] 브릿지 패턴(Bridge Pattern) 이해 및 예제

> 브릿지 패턴(Bridge Pattern)은 Flyweight 패턴, Adapter 패턴, Decorator 패턴처럼 구조 패턴 중 하나로, 두 인터페이스에 계층 구조(Hierarchy)를 가지고 있을 때 인터페이스를 구현(implements)으로부터 분리하고 클라이언트 프로그램으로부터 구현 세부사항을 숨기기 위해 사용되는 패턴입니다.

### 구조 패턴(Structural Pattern)이란?

구조 패턴이란 작은 클래스들을 상속과 합성을 이용하여 더 큰 클래스를 생성하는 방법을 제공하는 패턴입니다.

이 패턴을 사용하면 서로 독립적으로 개발한 클래스 라이브러리를 마치 하나인 양 사용할 수 있습니다. 또, 여러 인터페이스를 합성(Composite)하여 서로 다른 인터페이스들의 통일된 추상을 제공합니다.

구조 패턴의 중요한 포인트는 인터페이스나 구현을 복합하는 것이 아니라 객체를 합성하는 방법을 제공한다는 것입니다. 이는 **컴파일 단계에서가 아닌 런타임 단계에서 복합 방법이나 대상을 변경할 수 있다는 점에서 유연성을 갖습니다.**

### 브릿지 패턴 이해 및 예제

디자인 패턴의 교과서인 GoF에서는 브릿지 패턴에 대해 다음과 같이 정의하고 있습니다.

**추상화(abstraction)를 구현으로부터 분리하여 각각 독립적으로 변화할 수 있도록 하는 패턴**

예제를 통해 이해를 돕도록 하겠습니다.

예를 들어 다음과 같은 구조의 인터페이스를 구현하는 프로그램을 만든다고 가정해보겠습니다.

![image1](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FnrQF0%2FbtqIkuSK1sh%2F5pakKpU8Qk7aFi9S8aJsqK%2Fimg.png){: width="60%" height="60%"}

보시다시피 Shape 과 Color 라는 2가지 인터페이스가 있습니다.

이제 브릿지 패턴을 사용하여 두 인터페이스의 compoisition 을 구성해보도록 하겠습니다.

#### Color.java

```java
public interface Color {
 
	public void applyColor();
}
```

#### Shape.java

```java
public abstract class Shape {
	//Composition
	protected Color color;
	
	//constructor with implementor as input argument
	public Shape(Color c){
		this.color=c;
	}
	
	abstract public void applyColor();
}
```

Shape 클래스가 Color 인터페이스를 소유하고 있고, applyColor() 메소드는 abstract 로 선언하여 하위 클래스에게 구현을 위임합니다.

이번에는 이 Shape 추상 클래스를 상속하여 구체화 하는 Triangle 클래스와 Pentagon 클래스를 정의해보겠습니다.

#### Triangle.java

```java
public class Triangle extends Shape {
 
	public Triangle(Color c) {
		super(c);
	}
 
	@Override
	public void applyColor() {
		System.out.print("Triangle filled with color ");
		color.applyColor();
	} 
 
}
```

#### Pentagon.java

```java
public class Pentagon extends Shape {
 
	public Pentagon(Color c) {
		super(c);
	}
 
	@Override
	public void applyColor() {
		System.out.print("Pentagon filled with color ");
		color.applyColor();
	} 
 
}
```

마지막으로 각 Shape 클래스가 소유할 Color 인터페이스의 구현 객체를 정의해보겠습니다. 컬러는 RedColor 와 GreenColor 로 선언하겠습니다.

#### RedColor.java

```java
public class RedColor implements Color{
 
	public void applyColor(){
		System.out.println("red.");
	}
}
```

#### GreenColor.java

```java
public class GreenColor implements Color{
 
	public void applyColor(){
		System.out.println("green.");
	}
}
```

이제 위에서 정의한 클래스들을 사용하여 테스트 해보도록 하겠습니다.

#### BridgePatternTest.java

```java
public class BridgePatternTest {
 
	public static void main(String[] args) {
		Shape tri = new Triangle(new RedColor());
		tri.applyColor();
		
		Shape pent = new Pentagon(new GreenColor());
		pent.applyColor();
	}
}
```

#### 결과

```
Triangle filled with color red.
Pentagon filled with color green.
```

**브릿지 디자인 패턴은 추상화(abstraction)와 구현(implement)이 독립적으로 다른 계층 구조를 가질 수 있고, 클라이언트 어플리케이션으로부터 구현을 숨기고 싶을 때 사용될 수 있습니다.**

#### 원문링크

[[구조 패턴] 브릿지 패턴(Bridge Pattern) 이해 및 예제][link1]

[link1]: https://readystory.tistory.com/194?category=822867 "link1"

