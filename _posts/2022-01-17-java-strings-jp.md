---
layout: post
categories: "Java"
title: "Java String"
author: "TY_K"
---

## JAVA String、 String Buffer、 String Builder의 차이

앞서 이 클래스들의 공통점은 모두다<span style="color:blue">String(문자열)을 저장하고 관리하는 클래스</span>

### String vs StringBuffer, StringBuilder

String은 immutable(불변)하고 StringBuffer, StringBuilder는 mutable(가변)하다.

다시 말해서, String 클래스는 StringBuffer 클래스나 StringBuilder 클래스와 다르게 <span style="color:blue">리터럴을 통해 생성되면 그 인스턴스의 메모리 공간은 절대 변하지 않는다.</span>

```java
String literalString = "literal"; //리터럴로 생성하는 방식
String newString = new String("literal"); //new로 생성하는 방식

//위에서 "literal" 이라는 문자열을 String Pool에서 생성했기 때문에 이후에 추가한 str1, str2, str3는 추가적으로 생성하지않고 똑같은 문자열을 가리킨다

String str1 = "literal"; 
String str2 = "literal"; 
String str3 = "literal";
```

위 코드의 두 가지 String class 생성 방식에 따라 나뉘는데, 두 방식 모두 JVM 메모리 중 힙(heap) 영역에 생성된다는 점은 같다.

하지만 리터럴로 생성하면 특수하게<span style="color:blue">"String Pool"</span>이라는 공간에 생성된다. 이 메모리 공간에 생성된<span style="color:blue">문자열 값</span>은 절대 변하지 않는다는 얘기다.

그래서 '+' 연산이나 .concat() 메소드를 이용해서 문자열 값에 변화를 줘도 메모리 공간 내의 값이 변하는 것이 아니라, "String Pool"이라는 공간 안에 메모리를 할당받아 새로운 String 클래스 객체를 만들어서 문자열을 나타내는 것이다.

이렇게 새로운 문자열이 만들어지면 <span style="color:blue">'기존의 문자열'은 가비지 콜렉터에 의해 제거되야 하는 단점</span>(언제 제거될지 모름)이 있다.

또한 이러한 문자열 연산이 많아진다면, 연산 한 번 할 때마다<span style="color:blue">계속해서 문자열 객체를 만드는 오버헤드가 발생</span>발생하므로 성능이 떨어질 수 밖에 없다. (+ 연산에 내부적으로 char배열을 사용함)

대신 String 클래스의 객체는<span style="color:blue">불변</span>하기 때문에 단순하게 읽어가는 조회연산에서는 타 클래스보다 빠르게 읽을 수 있는 장점이 있다.
(추가 장점으로<span style="color:blue">불변</span>하기 때문에<span style="color:blue">멀티쓰레드환경에서 동기화를 신경쓸 필요가 없다.</span>)

### String 클래스가 적절한 경우

결론적으로, String 클래스는 문자열 연산이 적고, 자주 참조(조회)하는 경우에 사용하면 좋다. (특히 멀티 쓰레드 환경에서 신경쓸 게 없어서 좋다.)

### StringBuffer vs StringBuilder?

StringBuffer와 StringBuilder 클래스는 String과 다르게 mutable(변경가능)하다고 그랬다.

즉 문자열 연산을 할 때, 클래스는 한 번만 만들고(new), 메모리의 값을 변경시켜서 문자열을 변경한다.

그러므로 쓸데없이 중간에 문자열데이터들을 안 만들게 되니까<span style="color:blue">문자열 연산이 자주 있을 때 사용하면 성능이 좋다!</span>

심지어 StringBuffer와 StringBuilder 클래스의 메서드들이 같으므로 호환(?)이 가능하다.

그렇다면 StringBuffer와 StringBuilder의 차이는 무엇일까?

차이점은 StringBuffer는 멀티쓰레드환경에서 synchronized키워드가 가능하므로 동기화가 가능하다. 즉, thread-safe하다. 반대로 StringBuilder는 thread-safe하지 않다.

StringBuilder는 동기화를 지원하지 않기 때문에 멀티 쓰레드 환경에서는 적합하지 않다.

대신 StringBuilder가 동기화를 고려하지 않기 때문에 싱글쓰레드 환경에서 StringBuffer에 비해 연산처리가 빠른 장점이 있다.

### StringBuffer, StringBuilder가 적절한 경우

결론적으로, 문자열 연산이 많을 때 두 클래스를 사용하지만 멀티 쓰레드 환경에서는 StringBuffer를 사용하면 좋고, 싱글 쓰레드 환경이거나 멀티 쓰레드여도 굳이 동기화가 필요없는 경우에는 StringBuilder를 사용하는 것이 좋다.

### 정리

<span style="color:blue">String 클래스는 불변 객체</span>이기 때문에 문자열 연산이 많은 프로그래밍이 필요할 때 계속해서 인스턴스를 생성하므로 성능이 떨어지지만 조회가 많은 환경, 멀티쓰레드 환경에서 성능적으로 유리합니다.

<span style="color:blue">StringBuffer 클래스와 StringBuilder 클래스</span>는 문자열 연산이 자주 발생할 때 문자열이 변경가능한 객체기 때문에 성능적으로 유리합니다.

StringBuffer와 StringBuilder의 <span style="color:blue">차이점은 동기화 지원의 유무</span>이고 동기화를 고려하지 않는 환경에서 StringBuilder가 성능이 더 좋고, 동기화가 필요한 멀티쓰레드 환경에서는 StringBuffer를 사용하는 것이 유리합니다.

[참고링크][string]

[string]: https://jeong-pro.tistory.com/85 "string"