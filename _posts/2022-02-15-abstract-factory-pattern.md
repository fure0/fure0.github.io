---
layout: post
categories: "DesignPattern"
title: "Abstract Factory Pattern"
author: "TY_K"
---

### [생성 패턴] 추상 팩토리 패턴(Abstract Factory Pattern) 이해

이번에 살펴볼 디자인 패턴은 생성 패턴 중 하나인 **추상 팩토리 패턴(Abstract Factory Pattern)**입니다.

추상 팩토리 패턴은 이전 포스팅에서 살펴본 **팩토리 패턴(Factory Pattern)**과 유사한 패턴으로, 팩토리 of 팩토리라고도 볼 수 있습니다.

### 추상 팩토리 패턴은 생성 패턴(Creational Pattern) 중 하나이다.

생성 패턴은 인스턴스를 만드는 절차를 추상화하는 패턴입니다.

생성 패턴에 속하는 패턴들은 객체를 생성, 합성하는 방법이나 객체의 표현 방법을 시스템과 분리해줍니다.

생성 패턴은 시스템이 상속(inheritance) 보다 복합(composite) 방법을 사용하는 방향으로 진화되어 가면서 더 중요해지고 있습니다.

생성 패턴에서는 중요한 이슈가 두 가지 있습니다.

1. 생성 패턴은 시스템이 어떤 Concrete Class를 사용하는지에 대한 정보를 캡슐화합니다.
2. 생성 패턴은 이들 클래스의 인스턴스들이 어떻게 만들고 어떻게 결합하는지에 대한 부분을 완전히 가려줍니다.

쉬운 말로 정리하자면, 생성 패턴을 이용하면 무엇이 생성되고, 누가 이것을 생성하며, 이것이 어떻게 생성되는지, 언제 생성할 것인지 결정하는 데 유연성을 확보할 수 있게 됩니다.

### 추상 팩토리 패턴(Abstract Factory Pattern)이란?

팩토리 메소드 패턴에서는 하나의 팩토리 클래스가 인풋으로 들어오는 값에 따라 if-else나 switch 문을 사용하여 다양한 서브클래스를 리턴하는 형식으로 구현했었습니다.

**추상 팩토리 패턴에서는 팩토리 클래스에서 서브 클래스를 생성하는 데에 있어 이러한 if-else 문을 걷어냅니다.**

추상 팩토리 패턴은 **인풋으로** 서브클래스에 대한 식별 데이터를 받는 것이 아니라 또 하나의 팩토리 클래스를 받습니다.

처음에는 이 말이 무슨 말인지, 어떻게 팩토리 패턴과 다른지 헷갈릴 수 있습니다만 코드를 살펴보시면 훨씬 이해하기가 쉬우실 겁니다. 그럼 지금부터 예제를 통해 차근차근 살펴보도록 하겠습니다.

조금 더 면밀한 비교를 위해 팩토리 패턴에서 살펴본 예제와 동일한 클래스들을 사용해 예제를 작성하였습니다.

#### super class

```java
public abstract class Computer {
     
    public abstract String getRAM();
    public abstract String getHDD();
    public abstract String getCPU();
     
    @Override
    public String toString(){
        return "RAM= "+this.getRAM()+", HDD="+this.getHDD()+", CPU="+this.getCPU();
    }
}
```

#### sub class - 1

```java
public class PC extends Computer {
 
    private String ram;
    private String hdd;
    private String cpu;
     
    public PC(String ram, String hdd, String cpu){
        this.ram=ram;
        this.hdd=hdd;
        this.cpu=cpu;
    }
    @Override
    public String getRAM() {
        return this.ram;
    }
 
    @Override
    public String getHDD() {
        return this.hdd;
    }
 
    @Override
    public String getCPU() {
        return this.cpu;
    }
 
}
```

#### sub class - 2

```java
public class Server extends Computer {
 
    private String ram;
    private String hdd;
    private String cpu;
     
    public Server(String ram, String hdd, String cpu){
        this.ram=ram;
        this.hdd=hdd;
        this.cpu=cpu;
    }
    @Override
    public String getRAM() {
        return this.ram;
    }
 
    @Override
    public String getHDD() {
        return this.hdd;
    }
 
    @Override
    public String getCPU() {
        return this.cpu;
    }
 
}
```

자, 여기까지는 팩토리 패턴과 같습니다만 아래에서부터 차이가 납니다.

먼저 추상 팩토리의 역할을 하는 인터페이스 또는 추상 클래스가 필요합니다.

이번 예제에서는 인터페이스로 작성해보겠습니다.

```java
public interface ComputerAbstractFactory {
 
	public Computer createComputer();
 
}
```

여기서 주의 깊게 보실 것은 위에서 작성한 팩토리 인터페이스의 createComputer() 메소드의 리턴 타입이 super class인 Computer라는 것입니다. 이제 이 팩토리 인터페이스를 구현(implements)하는 클래스에서 createComputer() 메소드를 오버라이딩 하여 각각의 서브 클래스를 리턴해줄 것입니다. 이는 자바의 다형성을 아주 잘 활용한 방식이라 볼 수 있습니다.

#### PCFactory

```java
public class PCFactory implements ComputerAbstractFactory {
 
	private String ram;
	private String hdd;
	private String cpu;
	
	public PCFactory(String ram, String hdd, String cpu){
		this.ram=ram;
		this.hdd=hdd;
		this.cpu=cpu;
	}
	@Override
	public Computer createComputer() {
		return new PC(ram,hdd,cpu);
	}
 
}
```

#### ServerFactory

```java
public class ServerFactory implements ComputerAbstractFactory {
 
	private String ram;
	private String hdd;
	private String cpu;
	
	public ServerFactory(String ram, String hdd, String cpu){
		this.ram=ram;
		this.hdd=hdd;
		this.cpu=cpu;
	}
	
	@Override
	public Computer createComputer() {
		return new Server(ram,hdd,cpu);
	}
 
}
```

자, 이렇게 해서 두 sub class에 대한 팩토리 클래스인 PCFactory와 ServerFactory를 구현해보았습니다.

이제 마지막으로 이 서브 클래스들을 생성하기 위해 클라이언트 코드에 접점으로 제공되는 컨슈머 클래스(consumer class)를 만들어보겠습니다.

```java
public class ComputerFactory {
 
	public static Computer getComputer(ComputerAbstractFactory factory){
		return factory.createComputer();
	}
}
```

이제 클라이언트는 이 ComputerFacotry 클래스의 getComputer()라는 static 메소드에 앞서 구현한 PCFactory나 ServerFactory 인스턴스를 넣어줌으로써 **if-else** 없이도 각각 원하는 서브 클래스의 인스턴스를 생성할 수 있게 됐습니다.

한 번 사용해 보겠습니다.

```java
public class AbstractFactoryTest {
 
	public static void main(String[] args) {
		Computer pc = ComputerFactory.getComputer(new PCFactory("2 GB","500 GB","2.4 GHz"));
		Computer server = ComputerFactory.getComputer(new ServerFactory("16 GB","1 TB","2.9 GHz"));
		System.out.println("AbstractFactory PC Config::"+pc);
		System.out.println("AbstractFactory Server Config::"+server);
	}
    
}
```

```java
AbstractFactory PC Config::RAM= 2 GB, HDD=500 GB, CPU=2.4 GHz
AbstractFactory Server Config::RAM= 16 GB, HDD=1 TB, CPU=2.9 GHz
```

### 추상 팩토리 패턴의 장점

* **추상 팩토리 패턴**은 구현(Implements)보다 **인터페이스(Interface)를 위한 코드 접근법**을 제공합니다.
위 예에서 getComputer() 메소드는 파라미터로 인터페이스를 받아 처리를 하기 때문에 getComputer() 에서 구현할 것이 복잡하지 않습니다.
* **추상 팩토리 패턴**은 추후에 sub class를 확장하는 데 있어 굉장히 쉽게할 수 있습니다.
위 예에서 만약 Laptop 클래스를 추가하고자 한다면 getComputer()의 수정 없이 LaptopFactory만 작성해주면 됩니다.
이러한 특징에 기반하여 추상 팩토리 패턴은 **"Factory of Factories"**라고도 불립니다.
* **추상 팩토리 패턴**은 **팩토리 패턴(팩토리 메소드 패턴)의 조건문(if-else, switch 등)으로부터 벗어납니다.**

#### 원문링크

[[생성 패턴] 추상 팩토리 패턴(Abstract Factory Pattern) 이해 및 예제][link1]

[link1]: https://readystory.tistory.com/119?category=822867 "link1"