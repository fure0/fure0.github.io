---
layout: post
categories: "DesignPattern"
title: "Decorator pattern"
author: "TY_K"
---

### [구조 패턴] 데코레이터 패턴(Decorator Pattern) 이해

> 데코레이터 패턴(Decorator Pattern)은 Flyweight 패턴, Adapter 패턴, Bridge 패턴처럼 구조 패턴 중 하나로, 런타임에서 객체의 기능을 수정하는데 사용되는 패턴입니다.

### 구조 패턴(Structural Pattern)이란?

구조 패턴이란 작은 클래스들을 상속과 합성을 이용하여 더 큰 클래스를 생성하는 방법을 제공하는 패턴입니다.

이 패턴을 사용하면 서로 독립적으로 개발한 클래스 라이브러리를 마치 하나인 양 사용할 수 있습니다. 또, 여러 인터페이스를 합성(Composite)하여 서로 다른 인터페이스들의 통일된 추상을 제공합니다.

구조 패턴의 중요한 포인트는 인터페이스나 구현을 복합하는 것이 아니라 객체를 합성하는 방법을 제공한다는 것입니다. 이는 **컴파일 단계에서가 아닌 런타임 단계에서 복합 방법이나 대상을 변경할 수 있다는 점에서 유연성을 갖습니다.**

### 데코레이터 패턴 이해 및 예제

데코레이터 패턴은 상속(Inheritance)과 합성(Composition)을 사용하여 객체에 동적으로 책임을 추가할 수 있게 합니다. 이 방법은 서브 클래스를 생성하는 것보다 유연한 방법을 제공합니다.

예를 들어 다양한 종류의 자동차를 구현한다고 가정해보겠습니다. Car 라는 인터페이스를 정의하고 이를 구현하는 Basic Car 를 두고, 이를 확장하여 구현하는 Sports Car, Luxury Car 를 정의해보겠습니다.

그림으로 나타내면 다음과 같습니다.

![image1](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbkKEX1%2FbtqI7wBF8Ub%2Fgmd80zqVY2EdK7krMQ3nzk%2Fimg.png){: width="60%" height="60%"}

그런데 만약 런타임 단계에서 Sports Car의 특징과 Luxury Car의 특징을 모두 갖고 있는 Car 를 얻고 싶다면 어떻게 해야할까요? 일반적으로 둘의 기능을 갖는 또다른 서브 클래스를 정의할 수도 있겠습니다만, 데코레이터 패턴을 사용하여 해결해보도록 하겠습니다.

#### Car.java

```java
public interface Car {
	public void assemble();
}
```

#### BasicCar.java

```java
public class BasicCar implements Car {
 
	@Override
	public void assemble() {
		System.out.print("Basic Car.");
	}
}
```

#### CarDecorator.java

```java
public class CarDecorator implements Car {
 
	protected Car car;
	
	public CarDecorator(Car c){
		this.car=c;
	}
	
	@Override
	public void assemble() {
		this.car.assemble();
	}
}
```

#### SportsCar.java

```java
public class SportsCar extends CarDecorator {
 
	public SportsCar(Car c) {
		super(c);
	}
 
	@Override
	public void assemble(){
		super.assemble();
		System.out.print(" Adding features of Sports Car.");
	}
}
```

#### LuxuryCar.java

```java
public class LuxuryCar extends CarDecorator {
 
	public LuxuryCar(Car c) {
		super(c);
	}
	
	@Override
	public void assemble(){
		super.assemble();
		System.out.print(" Adding features of Luxury Car.");
	}
}
```

이제 모든 클래스가 준비되었으니 테스트 코드를 작성해보겠습니다.

#### DecoratorPatternTest.java

```java
public class DecoratorPatternTest {
 
	public static void main(String[] args) {
		Car sportsCar = new SportsCar(new BasicCar());
		sportsCar.assemble();
		System.out.println("\n*****");
		
		Car sportsLuxuryCar = new SportsCar(new LuxuryCar(new BasicCar()));
		sportsLuxuryCar.assemble();
	}
}
```

```
Basic Car. Adding features of Sports Car.
*****
Basic Car. Adding features of Luxury Car. Adding features of Sports Car.
```

보시다시피 CarDecorator 를 사용하여 SportsCar와 LuxuryCar 의 기능을 합성할 수 있게 됐습니다. 이렇게 작성할 경우 클래스의 종류가 많아질 수록 큰 효과를 발휘할 수 있는데요. 런타임에서 다양한 기능을 조합하여 기능을 사용할 수 있습니다.

#### 중요 포인트

* 데코레이터 패턴은 런타임에서 유연하게 객체의 기능들을 수정하고 조합하는데 유용하게 사용되는 패턴입니다.
* 단점이 있다면, 다수의 데코레이터 객체를 생성하고 사용해야 한다는 것입니다.
* JDK 에서 FileReader, BufferedReader 등 IO 클래스에 사용되는 패턴입니다.

#### 원문링크

[[구조 패턴] 데코레이터 패턴(Decorator Pattern) 이해 및 예제][link1]

[link1]: https://readystory.tistory.com/195?category=822867 "link1"