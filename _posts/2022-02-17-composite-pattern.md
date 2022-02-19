---
layout: post
categories: "DesignPattern"
title: "Composite pattern"
author: "TY_K"
---

### [구조 패턴] 복합체 패턴(Composite Pattern) 이해

**Composite 패턴은 구조 패턴 중 하나로, 객체들의 관계를 트리 구조로 구성하여 부분-전체 계층을 표현하는 패턴입니다. 사용자는 이 복합체 패턴을 통해 단일 객체와 복합 객체 모두 동일하게 다룰 수 있습니다.**

### 구조 패턴(Structural Pattern)이란?

구조 패턴이란 작은 클래스들을 상속과 합성을 이용하여 더 큰 클래스를 생성하는 방법을 제공하는 패턴입니다.

이 패턴을 사용하면 서로 독립적으로 개발한 클래스 라이브러리를 마치 하나인 양 사용할 수 있습니다. 또, 여러 인터페이스를 합성(Composite)하여 서로 다른 인터페이스들의 통일된 추상을 제공합니다.

구조 패턴의 중요한 포인트는 인터페이스나 구현을 복합하는 것이 아니라 객체를 합성하는 방법을 제공한다는 것입니다. 이는 **컴파일 단계에서가 아닌 런타임 단계에서 복합 방법이나 대상을 변경할 수 있다는 점에서 유연성을 갖습니다.**

### 복합체 패턴(Composite Pattern) 개념 및 예제 코드

복합체 패턴에 대해 실생활에서 예를 들어 설명해보겠습니다.

파워포인트에 삼각형, 사각형, 원을 각각 하나씩 만들어 놓고 삼각형과 사각형을 그룹화했다고 가정하겠습니다. 이제 우리는 모든 도형을 빨간색으로 색을 칠하려고 합니다.

**이때 우리가 채우기 버튼을 누를 때 선택하는 것이 어떤 도형인지, 혹은 그룹인지에 대해서 구분하지 않아도 됩니다. 파워포인트에서는 도형 하나에 대한 채우기와 그룹 전체에 대한 채우기 버튼이 같습니다.**

이처럼 복합체 패턴은 전체 도형들을 하나의 도형을 다루듯이 관리할 수 있다는 특징을 지닙니다.

쉽게 말해 **"일괄적인 관리"**가 가능한 것이지요.

복합체 패턴은 다음과 같은 오브젝트들을 갖습니다.

* Base Component - 베이스 컴포넌트는 클라이언트가 composition 내의 오브젝트들을 다루기 위해 제공되는 인터페이스를 말합니다. 베이스 컴포넌트는 인터페이스 또는 추상 클래스로 정의되며 모든 오브젝트들에게 공통되는 메소드를 정의해야 합니다.
* Leaf - composition 내 오브젝트들의 행동을 정의합니다. 이는 복합체를 구성하는 데 중요한 요소이며, 베이스 컴포넌트를 구현합니다. 그리고 Leaf는 다른 컴포넌트에 대해 참조를 가지면 안됩니다. 
* Composite - Leaf 객체들로 이루어져 있으며 베이스 컴포넌트 내 명령들을 구현합니다.

예제를 통해 이해를 돕도록 하겠습니다.

#### 1. Base Component

Base Component는 Leaf와 Composite의 공통되는 메소드들을 정의해야 합니다.

예제에서는 Shape 인터페이스 내에 각각 도형마다 색을 입히는 draw() 메소드를 정의하도록 하겠습니다.

#### Shape.java

```java
public interface Shape {
	
    public void draw(String fillColor);
}
```

#### 2. Leaf Objects

Leaf 객체들은 복합체에 포함되는 요소로, Base Component를 구현해야 합니다.

예제에서는 Triangle과 Circle 클래스를 정의하도록 하겠습니다.

#### Triangle.java

```java
public class Triangle implements Shape {
 
    @Override
    public void draw(String fillColor) {
        System.out.println("Drawing Triangle with color "+fillColor);
    }
}
```

#### Circle.java

```java
public class Circle implements Shape {
 
    @Override
    public void draw(String fillColor) {
        System.out.println("Drawing Circle with color "+fillColor);
    }
}
```

#### 3. Composite Objects

Composite 객체는 Leaf 객체들을 포함하고 있으며, Base Component를 구현할 뿐만 아니라 Leaf 그룹에 대해 add와 remove를 할 수 있는 메소드들을 클라이언트에게 제공합니다.

#### Drawing.java

```java
public class Drawing implements Shape {
 
    //collection of Shapes
    private List<Shape> shapes = new ArrayList<Shape>();
	
    @Override
    public void draw(String fillColor) {
        for(Shape sh : shapes) {
            sh.draw(fillColor);
        }
    }
	
    //adding shape to drawing
    public void add(Shape s) {
        this.shapes.add(s);
    }
	
    //removing shape from drawing
    public void remove(Shape s) {
        shapes.remove(s);
    }
	
    //removing all the shapes
    public void clear() {
        System.out.println("Clearing all the shapes from drawing");
        this.shapes.clear();
    }
}
```

여기서 중요한 것은 Composite Object 또한 Base Component를 구현해야 한다는 것입니다.

그렇게 해야만 클라이언트가 Composite 객체에 대해서 다른 Leaf들과 동일하게 취급될 수 있습니다.

아직까지 이해가 잘 안되더라도 괜찮습니다.

이제 우리가 만든 예제 코드를를 테스트하는 코드를 보면 확실하게 이해 가실 것입니다.

#### TestCompositePattern.java

```java
public class TestCompositePattern {
 
    public static void main(String[] args) {
        Shape tri = new Triangle();
        Shape tri1 = new Triangle();
        Shape cir = new Circle();
		
        Drawing drawing = new Drawing();
        drawing.add(tri1);
        drawing.add(tri1);
        drawing.add(cir);
		
        drawing.draw("Red");
		
        List<Shape> shapes = new ArrayList<>();
        shapes.add(drawing);
        shapes.add(new Triangle());
        shapes.add(new Circle());
        
        for(Shape shape : shapes) {
            shape.draw("Green");
        }
    }
}
```

```java
Drawing Triangle with color Red
Drawing Triangle with color Red
Drawing Circle with color Red
Drawing Triangle with color Green
Drawing Triangle with color Green
Drawing Circle with color Green
Drawing Triangle with color Green
Drawing Circle with color Green
```

자, 위 코드를 보시면 drawing 객체를 통해 Triangle, Circle 등의 Leaf 객체들을 Group으로 묶어서 한 번에 동작을 수행할 수 있습니다. 나아가 drawing 객체 또한 다른 도형들과 마찬가지로 Shape 인터페이스를 구현하고 있기 때문에 변수 shapes에서 살펴보는 것과 같이 drawing이 다른 도형들과 함께 취급될 수 있습니다.

#### 원문링크

[[구조 패턴] 복합체 패턴(Composite Pattern) 이해 및 예제][link1]

[link1]: https://readystory.tistory.com/131?category=822867 "link1"