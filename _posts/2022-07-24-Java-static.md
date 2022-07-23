---
layout: post
categories: "Java"
title: "Java Static"
author: "TY_K"
---

### 1. Static 정리

Java에서 Static 키워드를 사용한다는 것은 메모리에 한번 할당되어 프로그램이 종료될 때 해제되는 것을 의미합니다. 이를 정확히 이해하기 위해서는 메모리 영역에 대한 이해가 필요합니다.

#### Static의 메모리

![img1](https://t1.daumcdn.net/cfile/tistory/99AAAC405CEC82C032)

일반적으로 우리가 만든 **Class는 Static 영역에 생성**되고, **new 연산을 통해 생성한 객체는 Heap영역에 생성**됩니다. 객체의 생성시에 할당된 **Heap영역의 메모리는 Garbage Collector를 통해 수시로 관리**를 받습니다. 하지만 **Static 키워드를 통해 Static 영역에 할당된 메모리는 모든 객체가 공유하는 메모리**라는 장점을 지니지만, **Garbage Collector의 관리 영역 밖에 존재하므로 Static을 자주 사용하면 프로그램의 종료시까지 메모리가 할당된 채로 존재**하므로 자주 사용하게 되면 시스템의 퍼포먼스에 악영향을 주게 됩니다.

#### Static 변수 특징

- Static 변수는 클래스 변수이다.
- 객체를 생성하지 않고도 Static 자원에 접근이 가능하다.

Static 변수와 static 메소드는 Static 메모리 영역에 존재하므로 객체가 생성되기 이전에 이미 할당이 되어 있습니다. 그렇기 때문에 객체의 생성없이 바로 접근(사용)할 수 있습니다. 

```java
public class MyCalculator {

    public static String appName = "MyCalculator";
	    
	public static int add(int x, int y) {
	    return x + y;
	}

	public int min(int x, int y) {
	    return x - y;
	}
}

MyCalculator.add(1, 2);   //  static 메소드 이므로 객체 생성 없이 사용 가능
MyCalculator.min(1, 2);   //  static 메소드가 아니므로 객체 생성후에 사용가능

MyCalculator cal = new MyCalculator();
cal.add(1, 2);   // o 가능은 하지만 권장하지 않는 방법
cal.min(1, 2);   // o
```

### 2. Static 변수(정적 변수)

> 메모리에 고정적으로 할당되어, 프로그램이 종료될 때 해제되는 변수

Java에서 Static 변수는 메모리에 한번 할당되어 프로그램이 종료될 때 해제되는 변수로, 메모리에 한번 할당되므로 여러 객체가 해당 메모리를 공유하게 됩니다. 이해를 높이기 위해 코드를 추가하도록 하겠습니다.
예를 들어, 세상 모든 사람의 이름이 'MangKyu'인 세상에 살고있다고 가정을 하겠습니다. 이럴때면 우리는 아래와 같이 객체를 만들 수 있습니다.

```java
public class Person {
    private String name = "MangKyu";
	    
	public void printName() {
	    System.out.println(this.name);
	}
}
```

하지만 위와 같은 클래스를 통해 100명의 Person 객체를 생성하면, "MangKyu"라는 같은 값을 갖는 메모리가 100개나 중복해서 생성되게 됩니다. 이러한 경우에 static을 사용하여 여러 객체가 하나의 메모리를 참조하도록 하면 메모리 효율이 더욱 높아질 것입니다. 또한 "MangKyu"라는 이름은 결코 변하지 않는 값이므로 final 키워드를 붙여주며, 일반적으로 Static은 상수의 값을 갖는 경우가 많으므로 public으로 선언을 하여 사용합니다. 이러한 이유로, 일반적으로 static 변수는 public 및 final과 함께 사용되어 public static final로 활용 됩니다. 

```java
public class Person {

    public static final String name = "MangKyu";

    public static void printName() {
        System.out.println(this.name);
    }

}
```

### 3. Static 메소드(정적 메소드)

Static Method는 객체의 생성 없이 호출이 가능하며, 객체에서는 호출이 가능은 하지만 지양하고 있습니다. 일반적으로는 유틸리티 관련 함수들은 여러 번 사용되므로 static 메소드로 구현을 하는 것이 적합한데, static 메소드를 사용하는 대표적인 Util Class로는 java.uitl.Math가 있습니다. 우리는 해당 클래스를 아래와 같이 사용합니다.

```java
public class Test {

    private String name1 = "MangKyu";
    private static String name2 = "MangKyu";
 
    public static void printMax(int x, int y) {
        System.out.println(Math.max(x, y));
    }
         
    public static void printName(){
       // System.out.println(name1); 불가능한 호출
       System.out.println(name2);
    }

}
```

우리는 두 수의 최대값을 구하는 경우에 Math클래스를 사용하는데, static 메소드로 선언된 max 함수를 초기화 없이 사용합니다. 하지만 static 메소드에서는 static이 선언되지 않은 변수에 접근이 불가능한데, 메모리 할당과 연관지어 생각해보면 당연합니다. 우리가 Test.printName() 을 사용하려고 하는데, name1은 new 연산을 통해 객체가 생성된 후에 메모리가 할당됩니다. 하지만 static 메소드는 객체의 생성 없이 접근하는 함수이므로, 할당되지 않은 메모리 영역에 접근을 하므로 문제가 발생하게 됩니다. 그러므로 static 메소드에서 접근하기 위한 변수는 반드시 static 변수로 선언되어야 합니다.

### 4. 실제 Static 변수와 Static 메소드의 활용

1) Static 변수

일반적으로 상수들만 모아서 사용하며 상수의 변수명은 대문자와 _를 조합하여 이름짓는다. 또한 상속을 방지하기 위해 final class로 선언을 한다.

```java
public final class AppConstants {

    public static final String APP_NAME = "MyApp";
    public static final String PREF_NAME = "MyPref";

}
```

2) Static 메소드

마찬가지로 상속을 방지하기 위해 final class로 선언을 하고, 유틸 관련된 함수들을 모아둔다.

```java
import java.text.SimpleDateFormat;
import java.util.Date;
import android.util.Patterns;
 
public final class CommonUtils {
 
    public static String getCurrentDate() {
        Date date = new Date();
        SimpleDateFormat dateFormat = new SimpleDateFormat("yyMMdd");
        return dateFormat.format(date);
    }
     
    public static boolean isEmailValid(String email) {
        return Patterns.EMAIL_ADDRESS.matcher(email).matches();
    }
     
}
```

### <label style='color:orange'>어떤 경우에 왜 static을 사용해야하는지<label>

#### 1.클래스를 설계할 때, 멤버변수중 모든 인스턴스에 공통적으로 사용해야하는 것에 static을 붙인다.

인스턴스를 생성하면, 각 인스턴스들은 서로 독립적기 때문에 서로 다른 값을 유지한다. 경우에
따라서는 각 인스턴스들이 공통적으로 같은 값이 유지되어야 하는 경우 static을 붙인다.

#### 2. static이 붙은 멤버변수는 인스턴스를 생성하지 않아도 사용할수 있다.

static이 붙은 멤버변수(클래스변수)는 클래스가 메모리에 올라갈때 이미 자동적으로 생성되기 때문이다.

#### 3. static이 붙은 메서드(함수)에서는 인스턴스 변수를 사용할 수 없다.

static이 메서드는 인스턴스 생성 없이 호출가능한 반면, 인스턴스 변수는 인스턴스를 생성해야만
존재하기 때문에... static이 붙은 메서드(클래스메서드)를 호출할 때 인스턴스가 생성되어있을수도 그렇지 않을 수도 있어서 static이 붙은 메서드에서 인스턴스변수의 사용을 허용하지 않는다. (반대로, 인스턴스변수나 인스턴스메서드에서는 static이 붙은 멤버들을 사용하는
것이 언제나 가능하다. 인스턴스변수가 존재한다는 것은 static이 붙은
변수가 이미 메모리에 존재한다는 것을 의미하기 때문이다.)

#### 4. 메서드 내에서 인스턴스 변수를 사용하지 않는다면, static을 붙이는 것을 고려한다.

메서드의 작업내용중에서 인스턴스 변수를 필요로 한다면, static을 붙일 수 없다. 반대로 인스턴스변수를 필요로 하지 않는다면, 가능하면 static을 붙이는 것이 좋다. 메서드 호출시간이 짧아지기 때문에 효율이 높아진다. (static을 안붙인 메서드는 실행시 호출되어야할 메서드를 찾는 과정이 추가적으로 필요하기 때문에 시간이 더 걸린다.)

#### 5. 클래스 설계시 static의 사용지침

- 먼저 클래스의 멤버변수중 모든 인스턴스에 공통된 값을 유지해야하는 것이 있는지 살펴보고 있으면, static을 붙여준다.
- 작성한 메서드 중에서 인스턴스 변수를 사용하지 않는 메서드에 대해서 static을 붙일 것을 고려한다.

### <label style='color:orange'>Static 사용을 피해야 하는 이유?</label>

Java에는 static이라는 키워드가 존재하며, 이는 static으로 지시된 특정한 멤버가 해당 클래스의 인스턴스가 아니라 클래스 자체에 속해 있음을 나타냅니다. 즉, 클래스의 모든 인스턴스에서 공유되는 단 하나의 static member의 인스턴스가 생성되도록 명령하는 키워드입니다. 

![img2](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcR6SPu%2FbtqLhPtVAbM%2FwnWVUYM7GAkw7aOZHuehKK%2Fimg.jpg)

언뜻 보기에 static은 사용하기에도 편하고 효율적인 것처럼 보입니다. 예를 들어 어떤 클래스 내의 메서드를 매우 빈번하게 호출해야 한다면 그럴 때마다 인스턴스를 생성하는 것보다 Class.method()로 사용하는 것이 더 나아 보일 수도 있습니다. Stackoverflow의 **Why are static variables considered evil?** 에 따르면 아래와 같은 이유로 Static의 사용을 권장하지 않습니다. 

#### 1. 전체 프로그램과 동일한 라이프사이클

static 멤버는 사용을 하던 사용하지 않던 프로그램의 시작과 끝까지 메모리 내에 존재합니다. 

#### 2. static 변수의 생성과 소멸을 지시할 수 없음

프로그램이 로딩될 때 생성되고, 프로그램이 종료되거나 JVM이 내려갈 때 소멸됩니다. 개발자가 프로그램적으로 생성과 소멸에 관여할 수 없습니다. 

#### 3. thread safe하지 않음

프로그램 전역에서 사용되기 때문에 모든 스레드에서 static 필드를 공유하게 됩니다. 이때 한 스레드에서 값을 변경할 경우 다른 모든 스레드에서 영향을 받습니다. 이는 동시성 문제를 야기합니다. 

#### 4. thread safe하게 만들기 위해 추가적인 작업 필요

전역으로 관리되기 때문에 기본적으로 thread safety하지 않으며, synchronize를 사용하여 이를 보장하기 위해서는 추가적인 작업이 필요합니다. 

#### 5. static 메서드는 오버라이딩 불가 

#### 6. static 멤버는 Serialization 불가

객체 직렬화는 인스턴스에 대해 적용되기 때문에 클래스 자체 정보인 static 멤버는 여기에 포함되지 않습니다. 

#### 7. 런타임 다형성 불가

#### 8. 메모리 문제 

static 멤버는 프로그램이 종료될 때까지 Garbage Collector로 회수되지 않기 때문에, 많은 수의 static 필드나 메서드가 존재할 경우 메모리에 영향을 미칩니다. static 필드에 데이터가 계속해서 쌓이게 되면 OutOfMemory가 발생할 수도 있습니다. 

#### 9. 테스트하기 어려움 

static 필드는 전역으로 관리되기 때문에 프로그램 전체에서 이 필드에 접근할 수 있고 변경할 수 있으므로 해당 필드를 추론하기 어려워 테스트하기가 까다롭습니다. 

그러나 무조건적으로 static의 사용이 나쁘다는 것이 아닙니다. 늘 그렇듯이 올바르게 사용한다면 static은 그 자체로 성능을 향상할 수 있습니다.
예를 들어 자주 사용하지만 절대 변하지 않는 변수, 즉 상수의 경우에는 관례적으로 final static을 사용하여 선언합니다. 이 경우 적어도 1바이트 이상의 GC 대상 객체가 사라지게 됩니다. GC가 애초에 대상으로 인식하지 않기 때문에 성능 향상에 도움을 줄 수 있습니다. 
다만 단순히 편리하다는 이유로 잘못 사용하게 되면 의도한 바와 달리 예상하지 못한 문제를 야기할 수도 있습니다.


### <label style='color:orange'>Java8부터는 static이 heap영역에 저장된다?</label>

static은 런타임시 클래스 로더에 의해 메서드 영역에 적재되며 프로그램이 종료될 때 까지 GC에 대상이 아니라고 알고있었다. 그런데, permanent영역과 metaspace에 관련된 글을 읽는 중 static이 heap영역으로 할당된다는 말이있어 혼란스러웠다.

먼저 permanent영역과 metaspace영역에 대해서 정리해보자.

#### permanent영역

- permanent영역은 클래스 내부의 메타 데이터를 저장하는 영역이다.
- heap영역에 속하며 class, method meta data, static object, variable, constant pool등을 관리했다.
- java8이전에는 permanent영역은 method영역으로 사용되었다.
- java8이후부터 사라졌으며 metaspace영역으로 대체되었다.

#### metaspace영역

- java8부터 생긴 영역으로 permanent영역이 관리하던 정보를 저장한다.
- permanent영역과는 다르게 native memory영역으로서 jvm이 아닌 os에서 관리한다.
- method영역이 metaspace에 속한다.

일단, 여러 글을 읽으면서 permenent영역을 사람들마다 heap영역으로 보거나 heap영역이 아니라고 보는 관점이 있어 더 혼란스럽게 느껴졌다. permenent영역을 heap영역으로 본다면 static은 heap에서 관리한다는 말도 틀린 표현은 아니다. 하지만, 이 글에서는 permenent영역을 heap영역과 분리하는 관점에서 진행해보겠다.

우선 jdk8의 레퍼런스를 보면 아래와 같은 내용이 나온다.

![img3](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FmcNoh%2Fbtq5Szc2Ibz%2F1TqW4CvtQzxGWI8nQsdbJK%2Fimg.png)

![img4](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb0U7RP%2Fbtq5Vxscf3t%2FbZfYBeNXiks8sKcs15v931%2Fimg.png)

내용을 해석해보자면, hotspot jvm에서 permanent영역은 제거되고 permenent에서 관리하던 class metadata, interned String, class static variable은 heap영역이나 native memory영역으로 옮겨졌다. 더 정확한 표현으로 class meta-data는 native memory로 interned String과 class static은 heap영역으로 할당된다.

java8부터는 static을 heap영역에서 관리한다는 말이었다. 그렇다면 static을 사용해도 gc에 대상이되나 혼란스러웠다.
그런데, 글을 자세히 살펴보면 class statics, class static variables라는 표현을 사용한다. 즉, static object를 의미한다.
우선 static은 아래와 같은 방식으로 사용될 수 있다.

```java
static int i = 1; //1. primitive타입으로 선언된 static field
static void a(){} //2. 
static method static A a = new A(); //3. reference형식의 static field (static object)
```

primitive타입의 static variable, static method, reference형식의 Object는 permenent영역에 저장된다.

```java
static A a = new ArrayList<A>(); . . . a.add(new A()); a.add(new A()); a.add(new A()); . . .
```

문제는 위와 같이 collection 형태로 static object로서 생성되었고, 이후 collection에 객체가 새롭게 추가되는 상황이라면 permenent영역의 memory부족으로 OOM이 발생할 수 있다. 실제로 permenent영역에 메모리 부족문제는 종종 발생하던 이슈였다고 한다.

Java8부터는 permenent영역이 없어지고 metaspace가 생겼다. **기존에 perment영역에 저장되던 static object는 heap영역에 저장되도록 변경되었다(reference는 여전히 metaspace에서 관리된다).** 그렇기 때문에 참조를 잃은 static object는 GC의 대상이 될 수 있다.

결론은 Java8이전은 **Static Object**는 permanent영역, Java8버전부터 heap영역에서 관리된다.

#### 원문링크

[[Java] static변수와 static 메소드][link1]

[[Java] Static 키워드 바로 알고 사용하자][link2]

[Static 사용을 피해야 하는 이유][link3]

[[JAVA] Java8부터는 static이 heap영역에 저장된다?][link4]

[link1]: https://mangkyu.tistory.com/47 "link1"

[link2]: https://vaert.tistory.com/101 "link2"

[link3]: https://kellis.tistory.com/127 "link3"

[link4]: https://jgrammer.tistory.com/144 "link4"

