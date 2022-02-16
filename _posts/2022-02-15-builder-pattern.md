---
layout: post
categories: "DesignPattern"
title: "Builder pattern"
author: "TY_K"
---

### [생성 패턴] 빌더 패턴(Builder pattern) 이해

빌더 패턴은 **복잡한 객체를 생성하는 방법을 정의하는 클래스와 표현하는 방법을 정의하는 클래스를 별도로 분리하여, 서로 다른 표현이라도 이를 생성할 수 있는 동일한 절차를 제공하는 패턴**입니다. 빌더 패턴은 생성해야 되는 객체가 Optional한 속성을 많이 가질 때 빛을 발휘합니다.

### 빌더 패턴은 생성 패턴(Creational Pattern) 중 하나이다.

생성 패턴은 인스턴스를 만드는 절차를 추상화하는 패턴입니다.

생성 패턴에 속하는 패턴들은 객체를 생성, 합성하는 방법이나 객체의 표현 방법을 시스템과 분리해줍니다.

생성 패턴은 시스템이 상속(inheritance) 보다 복합(composite) 방법을 사용하는 방향으로 진화되어 가면서 더 중요해지고 있습니다.

생성 패턴에서는 중요한 이슈가 두 가지 있습니다.

1. 생성 패턴은 시스템이 어떤 Concrete Class를 사용하는지에 대한 정보를 캡슐화합니다.
2. 생성 패턴은 이들 클래스의 인스턴스들이 어떻게 만들고 어떻게 결합하는지에 대한 부분을 완전히 가려줍니다.
   
쉬운 말로 정리하자면, 생성 패턴을 이용하면 무엇이 생성되고, 누가 이것을 생성하며, 이것이 어떻게 생성되는지, 언제 생성할 것인지 결정하는 데 유연성을 확보할 수 있게 됩니다.

### 빌더 패턴(Builder Pattern)의 개념 및 예제

빌더 패턴은 많은 Optional한 멤버 변수(혹은 파라미터)나 지속성 없는 상태 값들에 대해 처리해야 하는 문제들을 해결합니다.

예를 들어, 팩토리 패턴이나 추상 팩토리 패턴에서는 생성해야하는 클래스에 대한 속성 값이 많을 때 아래와 같은 이슈들이 있습니다.

1. 클라이언트 프로그램으로부터 팩토리 클래스로 많은 파라미터를 넘겨줄 때 타입, 순서 등에 대한 관리가 어려워져 에러가 발생할 확률이 높아집니다.
2. 경우에 따라 필요 없는 파라미터들에 대해서 팩토리 클래스에 일일이 null 값을 넘겨줘야 합니다.
3. 생성해야 하는 sub class가 무거워지고 복잡해짐에 따라 팩토리 클래스 또한 복잡해집니다. 

빌더 패턴은 이러한 문제들을 해결하기 위해 별도의 Builder 클래스를 만들어 필수 값에 대해서는 생성자를 통해, 선택적인 값들에 대해서는 메소드를 통해 step-by-step으로 값을 입력받은 후에 build() 메소드를 통해 최종적으로 하나의 인스턴스를 리턴하는 방식입니다.

빌더 패턴은 굉장히 자주 사용되는 생성 패턴 중 하나로, **Retrofit**이나 **Okhttp** 등 유명 오픈소스에서도 이 빌더 패턴을 사용하고 있습니다.

빌더 패턴을 구현하는 방법은 아래와 같습니다.

1. 빌더 클래스를 Static Nested Class로 생성합니다. 이때, 관례적으로 생성하고자 하는 클래스 이름 뒤에 Builder를 붙입니다. 예를 들어, Computer 클래스에 대한 빌더 클래스의 이름은 ComputerBuilder 라고 정의합니다.
2. 빌더 클래스의 생성자는 public으로 하며, 필수 값들에 대해 생성자의 파라미터로 받습니다.
3. 옵셔널한 값들에 대해서는 각각의 속성마다 메소드로 제공하며, 이때 중요한 것은 메소드의 리턴 값이 빌더 객체 자신이어야 합니다.
4. 마지막 단계로, 빌더 클래스 내에 build() 메소드를 정의하여 클라이언트 프로그램에게 최종 생성된 결과물을 제공합니다. 이렇듯 build()를 통해서만 객체 생성을 제공하기 때문에 생성 대상이 되는 클래스의 생성자는 private으로 정의해야 합니다.

구현 방법만 봐서는 잘 이해가 가지 않을 수 있으니, 간단한 예제를 통해 이해를 돕도록 하겠습니다.

예제는 ComputerBuilder 클래스를 통해 Computer 클래스 객체를 생성하는 샘플 코드입니다. 

```java
public class Computer {
	
    //required parameters
    private String HDD;
    private String RAM;
	
    //optional parameters
    private boolean isGraphicsCardEnabled;
    private boolean isBluetoothEnabled;
	
 
    public String getHDD() {
        return HDD;
    }
 
    public String getRAM() {
        return RAM;
    }
 
    public boolean isGraphicsCardEnabled() {
        return isGraphicsCardEnabled;
    }
 
    public boolean isBluetoothEnabled() {
        return isBluetoothEnabled;
    }
	
    private Computer(ComputerBuilder builder) {
        this.HDD=builder.HDD;
        this.RAM=builder.RAM;
        this.isGraphicsCardEnabled=builder.isGraphicsCardEnabled;
        this.isBluetoothEnabled=builder.isBluetoothEnabled;
    }
	
    //Builder Class
    public static class ComputerBuilder{
 
        // required parameters
        private String HDD;
        private String RAM;
 
        // optional parameters
        private boolean isGraphicsCardEnabled;
        private boolean isBluetoothEnabled;
		
        public ComputerBuilder(String hdd, String ram){
            this.HDD=hdd;
            this.RAM=ram;
        }
 
        public ComputerBuilder setGraphicsCardEnabled(boolean isGraphicsCardEnabled) {
            this.isGraphicsCardEnabled = isGraphicsCardEnabled;
            return this;
        }
 
        public ComputerBuilder setBluetoothEnabled(boolean isBluetoothEnabled) {
            this.isBluetoothEnabled = isBluetoothEnabled;
            return this;
        }
		
        public Computer build(){
            return new Computer(this);
        }
 
    }
 
}
```

여기서 살펴볼 것은 **Computer 클래스가 setter 메소드 없이 getter 메소드만 가진다는 것과 public 생성자가 없다는 것입니다.** 그렇기 때문에 **Computer 객체를 얻기 위해서는 오직 ComputerBuilder 클래스를 통해서만 가능합니다.**

이제 이렇게 작성한 예제를 클라이언트에서 사용해보도록 하겠습니다.

```java
public class TestBuilderPattern {
 
    public static void main(String[] args) {
        Computer comp = new Computer.ComputerBuilder("500 GB", "2 GB")
                .setBluetoothEnabled(true)
                .setGraphicsCardEnabled(true)
                .build();
    }
 
}
```

보시는 것처럼 Computer 객체를 얻기 위해 ComputerBuilder 클래스를 사용하고 있으며 **필수 값인 HDD와 RAM 속성에 대해서는 생성자**로 받고 **Optional한 값인 BluetoothEnabled와 GraphicsCardEnabled에 대해서는 메소드를 통해 선택적**으로 입력 받고 있습니다.

즉, BluetoothEnabled 값이 필요 없는 객체라면 setBluetoothEnabled() 메소드를 사용하지 않으면 됩니다.

#### 원문링크

[[생성 패턴] 빌더 패턴(Builder pattern) 이해 및 예제][link1]

[link1]: https://readystory.tistory.com/121?category=822867 "link1"