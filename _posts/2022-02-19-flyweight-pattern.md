---
layout: post
categories: "DesignPattern"
title: "Flyweight pattern"
author: "TY_K"
---

### [구조 패턴] 플라이웨이트 패턴(Flyweight Pattern) 이해

> 플라이웨이트 패턴(Flyweight Pattern)은 Facade 패턴, Adapter 패턴, Decorator 패턴처럼 구조 패턴 중 하나로, 많은 수의 객체를 생성해야 할 때 사용하는 패턴입니다.

### 구조 패턴(Structural Pattern)이란?

구조 패턴이란 작은 클래스들을 상속과 합성을 이용하여 더 큰 클래스를 생성하는 방법을 제공하는 패턴입니다.

이 패턴을 사용하면 서로 독립적으로 개발한 클래스 라이브러리를 마치 하나인 양 사용할 수 있습니다. 또, 여러 인터페이스를 합성(Composite)하여 서로 다른 인터페이스들의 통일된 추상을 제공합니다.

구조 패턴의 중요한 포인트는 인터페이스나 구현을 복합하는 것이 아니라 객체를 합성하는 방법을 제공한다는 것입니다. 이는 **컴파일 단계에서가 아닌 런타임 단계에서 복합 방법이나 대상을 변경할 수 있다는 점에서 유연성을 갖습니다.**

### 플라이웨이트 패턴 이해 및 예제

**'공유(Sharing)'를 통하여 대량의 객체들을 효과적으로 지원하는 방법**

이처럼 플라이웨이트 패턴은 많은 수의 객체를 생성해야 할 때 주로 쓰입니다.

모든 객체는 저마다의 메모리를 할당받습니다. 따라서 모바일 기기나 임베디드 시스템 등 저용량 메모리를 지원하는 기기에서는 메모리를 관리하는 것이 아주 중요합니다.

플라이웨이트 패턴은 공유 객체에 의해 메모리에 로드 되는 객체의 개수를 줄일 수 있습니다.

플라이웨이트를 적용하기에 앞서 몇 가지 확인할 것이 있습니다.

아래 중 해당하는 사항이 많은 상황일수록 플라이웨이트 패턴을 적용하기에 적합합니다.

* 어플리케이션에 의해 생성되는 객체의 수가 많아야 한다.
* 생성된 객체가 오래도록 메모리에 상주하며, 사용되는 횟수가 많다.
* 객체의 특성을 내적 속성(Intrinsic Properties)과 외적 속성(Extrinsic Properties)으로 나눴을 때, 객체의 외적 특성이 클라이언트 프로그램으로부터 정의되어야 한다.

다른 건 어느 정도 알겠는데, 객체의 내적 속성과 외적 속성이라니 생소하게 느껴지셨을 겁니다.

하지만 플라이웨이트 패턴을 적용하기 위해서는 객체의 속성을 내적 속성과 외적 속성으로 나누어야 합니다.

그렇다면 내적 속성과 외적 속성은 무엇일까요?

**객체의 내적 속성은 객체를 유니크하게 하는 것이고, 외적 속성은 클라이언트의 코드로부터 설정되어 다른 동작을 수행하도록 사용되는 특성입니다. 예를 들어, Circle 이라는 객체는 color와 width라는 외적 속성을 가질 수 있습니다.**

자, 이제 플라이웨이트 패턴의 예제를 살펴볼 텐데, 그러기 위해서 우리는 Shared Objects를 리턴하는 Flyweight Factory를 생성해야 합니다. 이번 예제에서는 Line과 Oval을 통해 도형을 그리는 코드를 작성해보도록 하겠습니다.

우리는 먼저 Shape 이라는 인터페이스를 만들고, 그에 대한 구현 객체로 각각 Line과 Oval을 만들어주도록 하겠습니다.

이때 Oval과 Line에 각각 다른 특성을 부여할 건데, Oval은 주어진 색상으로 Oval을 채울지 결정하기 위한 내적 속성(Intrinsic Property)를 가질 것이고, Line은 그러한 내적 속성을 갖고 있지 않을 것입니다.

#### Shape.java

```java
import java.awt.Color;
import java.awt.Graphics;
 
public interface Shape {
 
    public void draw(Graphics g, int x, int y, int width, int height, Color color);
}
```

#### Line.java

```java
import java.awt.Color;
import java.awt.Graphics;
 
public class Line implements Shape {
 
    public Line(){
        System.out.println("Creating Line object");
        //adding time delay
        try {
            Thread.sleep(2000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
    @Override
    public void draw(Graphics line, int x1, int y1, int x2, int y2, Color color) {
        line.setColor(color);
        line.drawLine(x1, y1, x2, y2);
    }
 
}
```

#### Oval.java

```java
import java.awt.Color;
import java.awt.Graphics;
 
public class Oval implements Shape {
	
    //intrinsic property
    private boolean fill;
	
    public Oval(boolean f){
        this.fill = f;
        System.out.println("Creating Oval object with fill=" + f);
        //adding time delay
        try {
            Thread.sleep(2000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
    
    @Override
    public void draw(Graphics circle, int x, int y, int width, int height, Color color) {
        circle.setColor(color);
        circle.drawOval(x, y, width, height);
        if(fill){
            circle.fillOval(x, y, width, height);
        }
    }
 
}
```

여기서 눈여겨 보실 부분은 Line과 Oval 클래스의 생성자에 Thread.sleep을 적용했다는 것인데요, 이는 인스턴스화 할 때 시간이 많이 걸린다는 것을 조금 더 과장해서 보여주기 위해 넣은 코드입니다. 이를 통해 플라이웨이트 패턴을 쓰면 객체 생성에 있어서 얼마나 시간을 줄일 수 있는지 직관적으로 확인할 수 있습니다.

이번에는 위 객체들을 생성하는 데 사용되는 Flyweight Factory를 작성해보겠습니다.

플라이웨이트 팩토리는 클라이언트가 객체의 인스턴스를 생성하는 데 사용됩니다. 그래서 우리는 팩토리 안에 클라이언트는 접근할 수 없는 Map을 두어 객체들을 관리하도록 하겠습니다.

(만약 팩토리 패턴에 대해 모르시는 분들은 팩토리 패턴에 대해 다루었던 이전 포스팅을 참고하시면 되겠습니다)

클라이언트가 객체에 대한 인스턴스를 얻기 위해 호출할 때, 팩토리 안에 정의해둔 Map에 해당 객체가 있다면 별도의 인스턴스 생성 없이 그대로 Map에서 리턴하고, 만약 없다면 새로 인스턴스를 생성하여 맵에 저장(put) 한 후에 그 객체를 리턴하게 합니다.

플라이웨이트 팩토리 클래스는 아래와 같이 작성하겠습니다.

#### ShapeFactory.java

```java
import java.util.HashMap;
 
public class ShapeFactory {
 
    private static final HashMap<ShapeType, Shape> shapes = new HashMap<ShapeType, Shape>();
 
    public static Shape getShape(ShapeType type) {
        Shape shapeImpl = shapes.get(type);
 
        if (shapeImpl == null) {
            if (type.equals(ShapeType.OVAL_FILL)) {
                shapeImpl = new Oval(true);
            } else if (type.equals(ShapeType.OVAL_NOFILL)) {
                shapeImpl = new Oval(false);
            } else if (type.equals(ShapeType.LINE)) {
                shapeImpl = new Line();
            }
            shapes.put(type, shapeImpl);
        }
        return shapeImpl;
    }
	
    public static enum ShapeType {
        OVAL_FILL, OVAL_NOFILL, LINE;
    }
}
```

여기서 눈여겨 보실 것은 Map의 Key 값으로 ShapeType이라는 Enum을 사용한 것인데요, 이렇게 사용하면 타입 안전(Type-safety)하게 사용할 수 있습니다.

위 팩토리 클래스를 보시면 getShape() 메소드의 인자 값으로 들어오는 Enum 객체를 shapes라는 HashMap의 키 값으로 해서 이미 저장되어 있는 값인지 검사를 합니다.

**만약 맵에 객체가 있다면 별도의 인스턴스 생성 없이 맵에 들어있던 객체를 그대로 리턴하고, 맵에 객체가 없다면 새로운 인스턴스를 만들고 맵에 저장한 후에 생성한 객체를 리턴하게 됩니다.**

**이렇게 플라이웨이트 패턴을 사용하게 된다면 불필요한 인스턴스 생성을 최소화하기 때문에, 필요할 때마다 매번 새로운 인스턴스를 생성할 때보다 훨씬 메모리를 적게 사용하고, 빠른 프로그램을 작성할 수 있게 됩니다.**

이제 위 코드를 한 번 테스트 해보겠습니다.

#### DrawingClient.java

```java
import java.awt.BorderLayout;
import java.awt.Color;
import java.awt.Container;
import java.awt.Graphics;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
 
import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JPanel;
 
import com.journaldev.design.flyweight.ShapeFactory.ShapeType;
 
public class DrawingClient extends JFrame{
 
    private static final long serialVersionUID = -1350200437285282550L;
    private final int WIDTH;
    private final int HEIGHT;
 
    private static final ShapeType shapes[] = { ShapeType.LINE, ShapeType.OVAL_FILL,ShapeType.OVAL_NOFILL };
    private static final Color colors[] = { Color.RED, Color.GREEN, Color.YELLOW };
	
    public DrawingClient(int width, int height) {
        this.WIDTH = width;
        this.HEIGHT = height;
        Container contentPane = getContentPane();
 
        JButton startButton = new JButton("Draw");
        final JPanel panel = new JPanel();
 
        contentPane.add(panel, BorderLayout.CENTER);
        contentPane.add(startButton, BorderLayout.SOUTH);
        setSize(WIDTH, HEIGHT);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setVisible(true);
 
        startButton.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent event) {
                Graphics g = panel.getGraphics();
                for (int i = 0; i < 20; ++i) {
                    Shape shape = ShapeFactory.getShape(getRandomShape());
                    shape.draw(g, getRandomX(), getRandomY(), getRandomWidth(),
                            getRandomHeight(), getRandomColor());
                }
            }
        });
    }
	
    private ShapeType getRandomShape() {
        return shapes[(int) (Math.random() * shapes.length)];
    }
 
    private int getRandomX() {
        return (int) (Math.random() * WIDTH);
    }
 
    private int getRandomY() {
        return (int) (Math.random() * HEIGHT);
    }
 
    private int getRandomWidth() {
        return (int) (Math.random() * (WIDTH / 10));
    }
 
    private int getRandomHeight() {
        return (int) (Math.random() * (HEIGHT / 10));
    }
 
    private Color getRandomColor() {
        return colors[(int) (Math.random() * colors.length)];
    }
 
    public static void main(String[] args) {
        DrawingClient drawing = new DrawingClient(500,600);
    }
}
```

위 클라이언트 코드에서는 Shape의 타입을 랜덤으로 생성하게끔 작성했습니다.

만약 실행해보신다면, Line 객체와 Oval 객체가 **최초에 생성될 때에만 앞서 생성자에서 설정해두었던 sleep(2000)이 실행**되고 이후에는 별도의 딜레이 없이 빠르게 객체가 생성되는 것을 확인할 수 있습니다.

![image1](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FvUcud%2FbtqAkw6agh7%2F9HsmXJht4Gaj6RzJqNTKhk%2Fimg.png){: width="50%" height="50%"}

### 플라이웨이트 패턴 활용 예

플라이웨이트 패턴은 어디에서 쓰이고 있을까요?

**자바의 모든 래퍼 클래스의 valueOf() 메소드**가 바로 이 플라이웨이트 패턴을 사용하고 있습니다. 그래서 래퍼 클래스를 생성해야 할 때 new 키워드를 통해 인스턴스를 매번 생성하기보다는 valueOf() 메소드를 통해 생성하는 것이 더 효율적입니다.

또, 대표적으로 사용되는 것이 바로 **Java의 String Pool** 입니다. Java에서는 String Pool을 별도로 두어 같은 문자열에 대해 다시 사용될 때에 새로운 메모리를 할당하는 것이 아니라 String Pool에 있는지 검사해서 있으면 가져오고 없으면 새로 메모리를 할당하여 String Pool에 등록한 후에 사용하도록 하고 있습니다.

(이와 관련해서 자세한 내용은 추후에 별도로 포스팅을 하겠습니다.)

#### 원문링크

[[구조 패턴] 플라이웨이트 패턴(Flyweight Pattern) 이해 및 예제][link1]

[link1]: https://readystory.tistory.com/137?category=822867 "link1"