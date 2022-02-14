---
layout: post
categories: "DesignPattern"
title: "Factory Pattern"
author: "TY_K"
---

### [생성 패턴] 팩토리 패턴(Factory Pattern) 이해

이번에 살펴볼 디자인 패턴은 가장 유명한 디자인 패턴 중 하나인 **팩토리 패턴(Factory Pattern)**입니다.

이 팩토리 패턴은 조금 더 구체적인 용어인 **팩토리 메소드 패턴(Factory Method Pattern)**으로도 널리 알려져 있습니다.

### 팩토리 패턴은 생성 패턴(Creational Pattern) 중 하나이다.

생성 패턴은 인스턴스를 만드는 절차를 추상화하는 패턴입니다.

생성 패턴에 속하는 패턴들은 객체를 생성, 합성하는 방법이나 객체의 표현 방법을 시스템과 분리해줍니다.

생성 패턴은 시스템이 상속(inheritance) 보다 복합(composite) 방법을 사용하는 방향으로 진화되어 가면서 더 중요해지고 있습니다.

생성 패턴에서는 중요한 이슈가 두 가지 있습니다.

1. 생성 패턴은 시스템이 어떤 Concrete Class를 사용하는지에 대한 정보를 캡슐화합니다.
2. 생성 패턴은 이들 클래스의 인스턴스들이 어떻게 만들고 어떻게 결합하는지에 대한 부분을 완전히 가려줍니다.

쉬운 말로 정리하자면, 생성 패턴을 이용하면 무엇이 생성되고, 누가 이것을 생성하며, 이것이 어떻게 생성되는지, 언제 생성할 것인지 결정하는 데 유연성을 확보할 수 있게 됩니다.

### 팩토리 패턴이란?

팩토리 패턴은 객체를 생성하는 인터페이스는 미리 정의하되, 인스턴스를 만들 클래스의 결정은 서브 클래스 쪽에서 내리는 패턴입니다. 다시 말해 **여러 개의 서브 클래스를 가진 슈퍼 클래스가 있을 때 인풋에 따라 하나의 자식 클래스의 인스턴스를 리턴해주는 방식**입니다.

팩토리 패턴에서는 클래스의 인스턴스를 만드는 시점을 서브 클래스로 미룹니다.

이 패턴은 인스턴스화에 대한 책임을 객체를 사용하는 클라이언트에서 팩토리 클래스로 가져옵니다.

### 활용성

* 어떤 클래스가 자신이 생성해야 하는 객체의 클래스를 예측할 수 없을 때
* 생성할 객체를 기술하는 책임을 자신의 서브클래스가 지정했으면 할 때

### Java Example

팩토리 패턴에 사용되는 슈퍼 클래스는 인터페이스나 추상 클래스, 혹은 그냥 평범한 자바 클래스여도 상관없습니다.

그러나 이번 예제에서는 추상 클래스로 만들어 toString()을 오버라이딩 하여 코드를 작성해보도록 하겠습니다.

#### Super Class

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

슈퍼 클래스를 만들었으니 이번에는 PC와 Server라는 이름의 두 서브 클래스를 만들어 보겠습니다.

#### Sub Class - 1

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

#### Sub Class - 2

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

여기서 주의할 점은 PC 클래스와 Server 클래스 모두 Computer 클래스를 상속한다는 것입니다.

자, 이제 마지막으로 팩토리 클래스를 만들어보도록 하겠습니다.

#### Factory Class

```java
public class ComputerFactory {
 
    public static Computer getComputer(String type, String ram, String hdd, String cpu){
        if("PC".equalsIgnoreCase(type))
            return new PC(ram, hdd, cpu);
        else if("Server".equalsIgnoreCase(type))
            return new Server(ram, hdd, cpu);
		
        return null;
    }
}
```

자, 이제 코드를 살펴보겠습니다. ComputerFactory 클래스의 getComputer 메소드를 살펴보면 **static 메소드**로 구현되었다는 점을 살펴볼 수 있고, 메소드 내부 코드를 보면 type의 값이 "PC"일 경우 PC 클래스의 인스턴스를, "Server"일 경우 Server 클래스의 인스턴스를 리턴하는 것을 볼 수 있습니다.

**이렇듯 팩토리 메소드 패턴을 사용하게 된다면 인스턴스를 필요로 하는 Application에서 Computer의 Sub 클래스에 대한 정보는 모른 채 인스턴스를 생성할 수 있게 됩니다.**

이렇게 구현한다면 앞으로 Computer 클래스에 더 많은 Sub 클래스가 추가된다 할지라도 getComputer()를 통해 인스턴스를 제공받던 Application의 코드는 수정할 필요가 없게 됩니다.

팩토리 메소드 패턴을 구현하는 데 중요한 점이 두 가지가 있습니다.

1. Factory class를 **Singleton**으로 구현해도 되고, 서브클래스를 리턴하는 static 메소드로 구현해도 됩니다.
2. 팩토리 메소드는 위 예제의 getComputer()와 같이 입력된 파라미터에 따라 다른 서브 클래스의 인스턴스를 생성하고 리턴합니다.

마지막으로, 위 예제에서 작성한 ComputerFactory 클래스를 사용하여 PC와 Server 클래스의 인스턴스를 생성해보겠습니다.

```java
public class TestFactory {
 
    public static void main(String[] args) {
        Computer pc = ComputerFactory.getComputer("pc","2 GB","500 GB","2.4 GHz");
        Computer server = ComputerFactory.getComputer("server","16 GB","1 TB","2.9 GHz");
        System.out.println("Factory PC Config::"+pc);
        System.out.println("Factory Server Config::"+server);
    }
 
}
```

```java
Factory PC Config::RAM= 2 GB, HDD=500 GB, CPU=2.4 GHz
Factory Server Config::RAM= 16 GB, HDD=1 TB, CPU=2.9 GHz
```

### 팩토리 패턴의 장점

1. 팩토리 패턴은 클라이언트 코드로부터 서브 클래스의 인스턴스화를 제거하여 서로 간의 종속성을 낮추고, 결합도를 느슨하게 하며(Loosely Coupled), 확장을 쉽게 합니다.
예를 들어, 위 예제에서 작성한 클래스 중 PC class에 대해 수정 혹은 삭제가 일어나더라도 클라이언트는 알 수 없기 때문에 코드를 변경할 필요도 없습니다.
2. 팩토리 패턴은 클라이언트와 구현 객체들 사이에 추상화를 제공합니다.

### 사용 예

1. java.util 패키지에 있는 Calendar, ResourceBundle, NumberFormat 등의 클래스에서 정의된 **getInstance()** 메소드가 팩토리 패턴을 사용하고 있습니다.
2. Boolean, Integer, Long 등 Wrapper class 안에 정의된 **valueOf()** 메소드 또한 팩토리 패턴을 사용했습니다.

#### 원문링크

[[생성 패턴] 팩토리 패턴(Factory Pattern) 이해 및 예제][link1]

[link1]: https://readystory.tistory.com/117?category=822867 "link1"