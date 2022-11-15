---
layout: post
categories: "Java"
title: "Java Nested Class"
author: "TY_K"
---

### Nested Class

- 한 곳에서만 사용되는 클래스를 논리적으로 묶어서 처리할 필요가 있을 때 → Static Nested 클래스 사용
- 캡슐화가 필요할 때. 즉, 내부 구현을 감추고 싶을 때 → inner 클래스 사용

  예를 들어 A라는 클래스에 private 변수가 있다. 이 변수에 접근하고 싶은 B라는 클래스를 선언하고, B 클래스를 외부에 노출시키고 싶지 않을 경우

- Static nested 클래스, inner 클래스 차이는 static 여부이다.

#### Static Nested Class

```java
public class OuterOfStatic{
  static class StaticNested {
    private int value = 0;
    public int getValue() {
      return value;
    }
    public void setValue(int value){
      this.value=value;
    }
  }
}
```
```java
public class NestedSample{
  public static void main(String[] args){
    NestedSample sample = new NestedSample();
    sample.makeStaticNestedObject();
  }
  public void makeStaticNestedObject(){
    OuterOfStatic.StaticNested staticNested = new OuterOfStatic.StaticNested();
    staticNested.setValue(3);
    System.out.println(staticNested.getValue());
  }
}
```
- Static Nested 클래스를 만들었을 때 객체 생성은 감싸고 있는 클래스 이름 뒤에 .을 찍고 쓰면된다.

#### Inner Class

```java
public class OuterOfInner {
  class Inner {
    private int value = 0;
    public int getValue() {
      return value
    }
    public void setValue(int value){
      this.value=value;
    }
  }
}
```
```java
public class InnerSample{
  public static void main(String[] args){
    InnerSample sample = new InnerSample();
    sample.makeInnerObject();
  }
  public void makeInnerObject(){
    OuterOfInner outer = new OuterOfInner();
    OuterOfInner.Inner inner = outer.newInner();
    inner.setValue(3);
    System.out..println(inner.getValue());
  }
}
```
- Inner클래스의 객체를 만들기 위해서 먼저 Inner 클래스를 감싸고 있는 클래스의 객체를 만들어야한다.

### 자바파일 한개당 public 클래스는 하나만 가질수 있고 그 클래스는 파일명이랑 똑같은 클래스 여야된다.

example.java라는 파일이 있다고 하면

```java
class example{
}
public class example2{
}
```
이렇게 하면 컴파일이 안된다.
```java
public class example{
}
class example2{
}
```
이렇게 바꾸면 컴파일이 된다.

#### 원문링크

[자바의 신 16장 - 클래스 안에 클래스가 들어갈 수도 있구나][link1]

[link1]: https://velog.io/@always/%EC%9E%90%EB%B0%94%EC%9D%98-%EC%8B%A0-16%EC%9E%A5-%ED%81%B4%EB%9E%98%EC%8A%A4-%EC%95%88%EC%97%90-%ED%81%B4%EB%9E%98%EC%8A%A4%EA%B0%80-%EB%93%A4%EC%96%B4%EA%B0%88-%EC%88%98%EB%8F%84-%EC%9E%88%EA%B5%AC%EB%82%98 "link1"

[[Java] Nested Class(클래스 안에 클래스)][link2]

[link2]: https://onsil-thegreenhouse.github.io/programming/java/2017/12/12/java_tutorial_1-16/ "link2"

[java public class 하나만 가질 수 있는 이유][link3]

[link3]: https://velog.io/@sungmo738/java-public-class-%ED%95%98%EB%82%98%EB%A7%8C-%EA%B0%80%EC%A7%88-%EC%88%98-%EC%9E%88%EB%8A%94-%EC%9D%B4%EC%9C%A0 "link3"