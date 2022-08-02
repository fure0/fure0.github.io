---
layout: post
categories: "Java"
title: "Java Compare"
author: "TY_K"
---

<style>
    .post img {
        margin : 0
    }
</style>

### Comparable과 Comparator의 차이

Comparable과 Comparator는 모두 **인터페이스(interface)**라는 것이다.

즉, Comparable 혹은 Comparator을 사용하고자 한다면 인터페이스 내에 선언된 메소드를 **'반드시 구현'**해야한다는 것이다.

Comparable 인터페이스에는 **compareTo(T o)** 메소드 하나가 선언되어있는 것을 볼 수 있다. 

이 말은 우리가 만약 Comparable을 사용하고자 한다면 compareTo 메소드를 재정의(Override/구현)을 해주어야 한다는 것이다.

Comparator를 보면 선언 된 메소드가 많아서 어질할 수 있겠지만, 우리가 실질적으로 구현해야 하는 것은 단 하나다.

바로 **compare(T o1, T o2)** 다.

여기서 하나 의문이 들 수 있다. 

> "인터페이스 내에 선언된 메소드를 **'반드시 구현'**해야한다고 말했는데 왜 compare 메소드만 구현하는 거죠?"

Java8부터는 Interface에서도 일반 메소드(함수)를 구현할 수 있도록 변경되었다. 

보면 알겠지만, 거의 대부분이 default 로 선언된 메소드나, static으로 선언된 메소드인 것을 볼 수 있다.

그리고 default 나 static으로 선언된 메소드들을 보면 함수를 구현하고 있다.

![img1](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdonyzB%2Fbtq3zM6CXiO%2FeCX7EIRQglpytqapalnook%2Fimg.png){: width="80%" height="80%"}
[interface Comparato]

보다시피 기존 구현된 함수를 반환하거나 등, 함수의 내용이 { ... } 블럭 안에 구현이 되어있는 것을 볼 수 있다.

이 말은, default나 static으로 선언된 메소드가 아니면 이는 추상메소드라는 의미로 반드시 재정의를 해주어야 한다는 것이다.

여담으로 default와 static의 차이라면 default로 선언된 메소드는 재정의(Override)를 할 수 있고, static은 재정의를 할 수 없다는 차이다.

### Comparable과 Comparator의 역할은 비슷한 것 같은데 무슨 차이인 것일까?

왜 Comparable의 compareTo(T o) 메소드는 파라미터(매개변수)가 한 개이고, Comparator의 compare(T o1, T o2) 메소드는 파라미터가 왜 두 개인 것일까?

일단, 두 인터페이스를 구체적으로 알아보기에 앞서 먼저 정답부터 말하자면, Comparable은 **"자기 자신과 매개변수 객체를 비교"**하는 것이고, Comparator는 **"두 매개변수 객체를 비교"**한다는 것이다.

쉽게 말하자면, Comparable은 자기 자신과 파라미터로 들어오는 객체를 비교하는 것이고, Comparator는 자기 자신의 상태가 어떻던 상관없이 파라미터로 들어오는 두 객체를 비교하는 것이다. 즉, **본질적으로 비교한다는 것 자체는 같지만, 비교 대상이 다르다는 것**이다.

### Comparable

**"자기 자신과 매개변수 객체를 비교"**한다고 했다. 

```java
class Student implements Comparable<Student> {
 
   int age;         // 나이
   int classNumber;   // 학급
   
   Student(int age, int classNumber) {
      this.age = age;
      this.classNumber = classNumber;
   }
   
   @Override
   public int compareTo(Student o) {
    
      // 자기자신의 age가 o의 age보다 크다면 양수
      if(this.age > o.age) {
         return 1;
      }
      // 자기 자신의 age와 o의 age가 같다면 0
      else if(this.age == o.age) {
         return 0;
      }
      // 자기 자신의 age가 o의 age보다 작다면 음수
      else {
         return -1;
      }
   }
}
```

"자기 자신"과 "상대방"을 비교하는 것이다. 즉, 자기 자신을 기준으로 삼아 대소관계를 파악해야 한다.

만약 내가 갖고 있는 값이 7라고 가정해보자. 그리고 상대방은 3이라고 가정한다면, 나 자신은 상대방보다 값이 4만큼 크다.

반대로 상대방이 9을 갖고 있다고 가정하면, 나는 상대방보다 2만큼 작다. 즉, -2 만큼 크다는 것이다.

이를 좀 더 생각해보면 -1, 0, 1로 반환할 수도 있으나, 그냥 두 비교대상의 값 차이를 반환해도 되지 않겠는가?

```java
class Student implements Comparable<Student> {
 
   int age;         // 나이
   int classNumber;   // 학급
   
   Student(int age, int classNumber) {
      this.age = age;
      this.classNumber = classNumber;
   }
   
   @Override
   public int compareTo(Student o) {
 
      /*
       * 만약 자신의 age가 o의 age보다 크다면 양수가 반환 될 것이고,
       * 같다면 0을, 작다면 음수를 반환할 것이다.
       */
      return this.age - o.age;
   }
}
```

### 테스트

```java
public class Test {
   public static void main(String[] args)  {
 
      Student a = new Student(17, 2);   // 17살 2반
      Student b = new Student(18, 1);   // 18살 1반
      
      
      int isBig = a.compareTo(b);   // a자기자신과 b객체를 비교한다.
      
      if(isBig > 0) {
         System.out.println("a객체가 b객체보다 큽니다.");
      }
      else if(isBig == 0) {
         System.out.println("두 객체의 크기가 같습니다.");
      }
      else {
         System.out.println("a객체가 b객체보다 작습니다.");
      }
      
   }
 
}
 
class Student implements Comparable<Student> {
 
   int age;         // 나이
   int classNumber;   // 학급
   
   Student(int age, int classNumber) {
      this.age = age;
      this.classNumber = classNumber;
   }
   
   @Override
   public int compareTo(Student o) {
      return this.age - o.age;
   }
}

// "a객체가 b객체보다 작습니다."
```

### 주의해야 할 점이 있다

사실 우리가 편리하게 두 수의 대소비교를 두 수의 차를 통해 음수, 0, 양수로 구분하여 구했지만, 여기에는 치명적인 단점이 있다. 바로 뺄셈 과정에서 자료형의 범위를 넘어버리는 경우가 발생할 수 있기 때문이다.

일단 위 예시에선 int형으로 살펴보았으니 이를 예로 들겠다.

먼저 int형의 범위가 어떤지를 파악해야 한다.

int 자료형은 32비트(4바이트) 자료형이며 표현 범위가 -231 ~ 231-1 으로, 이를 풀어쓰면 -2,147,483,648 ~ 2,147,483,647 이다.

만약 해당 범위 밖을 넘게 되면 반대편의 값으로 넘어가게 된다. 

쉽게 말하자면 -2,147,483,648 - 1 = -2,147,483,649 일 것이다. 하지만, int 자료형에서 표현할 수 없는 수로 2,147,483,647으로 int형의 최댓값으로 반환한다. 이렇게 주어진 범위의 하한선을 넘어버리는 것을 **'Underflow'** 라고 한다.

반대로 2,147,483,647 + 1 = 2,147,483,648 일 것이다. 하지만 마찬가지로 int 자료형에서 표현할 수 없는 수로 -2,147,483,648 이 되어 int 형의 최솟값으로 반환된다. 이렇게 주어진 범위의 상한선을 넘어버리는 것을 **'Overflow'** 라고 한다.

compareTo를 구현하거나, 이후 설명 할 compare을 구현 할 때 대소비교에 있어 이러한 Overflow 혹은, Underflow가 발생할 여지가 있는지를 반드시 확인하고 사용해야 한다.

### Comparator

**"두 매개변수 객체를 비교"**한다고 했다.

이 말은 자기 자신이 아니라 파라미터(매개 변수)로 들어오는 두 객체를 비교하는 것이다. 여기서 바로 Comparable과 차이가 발생하는 것이다.

**기본적으로 compare메소드 매커니즘 자체는 compareTo와 같다.**

다만, **자기 자신과 비교되느냐 안되느냐의 차이**일 뿐이다. 

일단 코드를 보면서 이해해보자. 이 번엔 한 번 학급을 기준으로 정의를 해보도록 하겠다.

```java
import java.util.Comparator;   // import 필요
class Student implements Comparator<Student> {
 
   int age;         // 나이
   int classNumber;   // 학급
   
   Student(int age, int classNumber) {
      this.age = age;
      this.classNumber = classNumber;
   }
   
   @Override
   public int compare(Student o1, Student o2) {
    
      // o1의 학급이 o2의 학급보다 크다면 양수
      if(o1.classNumber > o2.classNumber) {
         return 1;
      }
      // o1의 학급이 o2의 학급과 같다면 0
      else if(o1.classNumber == o2.classNumber) {
         return 0;
      }
      // o1의 학급이 o2의 학급보다 작다면 음수
      else {
         return -1;
      }
   }
}
```

앞서 Comparable에서 했던 것처럼 간략하게 할 수 있다.

```java
import java.util.Comparator;   // import 필요
class Student implements Comparator<Student> {
 
   int age;         // 나이
   int classNumber;   // 학급
   
   Student(int age, int classNumber) {
      this.age = age;
      this.classNumber = classNumber;
   }
   
   @Override
   public int compare(Student o1, Student o2) {
 
      /*
       * 만약 o1의 classNumber가 o2의 classNumber보다 크다면 양수가 반환 될 것이고,
       * 같다면 0을, 작다면 음수를 반환할 것이다.
       */
      return o1.classNumber - o2.classNumber;
   }
}
```

### 테스트

```java
import java.util.Comparator;
 
public class Test {
   public static void main(String[] args)  {
 
      Student a = new Student(17, 2);   // 17살 2반
      Student b = new Student(18, 1);   // 18살 1반
      Student c = new Student(15, 3);   // 15살 3반
         
      // a객체와는 상관 없이 b와 c객체를 비교한다.
      int isBig = a.compare(b, c);
      
      if(isBig > 0) {
         System.out.println("b객체가 c객체보다 큽니다.");
      }
      else if(isBig == 0) {
         System.out.println("두 객체의 크기가 같습니다.");
      }
      else {
         System.out.println("b객체가 c객체보다 작습니다.");
      }
      
   }
}
 
class Student implements Comparator<Student> {
 
   int age;         // 나이
   int classNumber;   // 학급
   
   Student(int age, int classNumber) {
      this.age = age;
      this.classNumber = classNumber;
   }
   
   @Override
   public int compare(Student o1, Student o2) {
      return o1.classNumber - o2.classNumber;
   }
}

// b객체가 c객체보다 작습니다.
```

### Comparator 활용편

위에서 보았듯이 Comparator를 통해 compare 메소드를 사용려면 결국에는 compare메소드를 활용하기 위한 객체가 필요하게 된다.

무슨 말인가 하면, a, b, c 객체가 생성되어있고, 이들을 비교를 하고 싶다면 어느 한 객체를 통해 compare메소드를 사용해야한다는 것이다.

Comparator 기능만 따로 두고싶다면 어떻게 해야할까?

답은 매우 간단하다.

**"익명 객체(클래스)를 활용한다"**

```java
import java.util.Comparator;
 
public class Test {
   public static void main(String[] args) {
    
      // 익명 객체 구현방법 1
      Comparator<Student> comp1 = new Comparator<Student>() {
         @Override
         public int compare(Student o1, Student o2) {
            return o1.classNumber - o2.classNumber;
         }
      };
   }
 
   // 익명 객체 구현 2
   public static Comparator<Student> comp2 = new Comparator<Student>() {
      @Override
      public int compare(Student o1, Student o2) {
         return o1.classNumber - o2.classNumber;
      }
   };
}
 
 
// 외부에서 익명 객체로 Comparator가 생성되기 때문에 클래스에서 Comparator을 구현 할 필요가 없어진다.
class Student {
 
   int age;         // 나이
   int classNumber;   // 학급
   
   Student(int age, int classNumber) {
      this.age = age;
      this.classNumber = classNumber;
   }
 
}
```

### 테스트

```java
import java.util.Comparator;
 
public class Test {
   public static void main(String[] args)  {
 
      Student a = new Student(17, 2);   // 17살 2반
      Student b = new Student(18, 1);   // 18살 1반
      Student c = new Student(15, 3);   // 15살 3반
         
      // comp 익명객체를 통해 b와 c객체를 비교한다.
      int isBig = comp.compare(b, c);
      
      if(isBig > 0) {
         System.out.println("b객체가 c객체보다 큽니다.");
      }
      else if(isBig == 0) {
         System.out.println("두 객체의 크기가 같습니다.");
      }
      else {
         System.out.println("b객체가 c객체보다 작습니다.");
      }
      
   }
   
   public static Comparator<Student> comp = new Comparator<Student>() {
      @Override
      public int compare(Student o1, Student o2) {
         return o1.classNumber - o2.classNumber;
      }
   };
}
 
class Student {
 
   int age;         // 나이
   int classNumber;   // 학급
   
   Student(int age, int classNumber) {
      this.age = age;
      this.classNumber = classNumber;
   }
   
}

// b객체가 c객체보다 작습니다.
```

### Comparable, Comparator 와 정렬의 관계

이제 Comparable과 Comparator의 각각의 차이점과 사용 방법을 이해했을 것이다.

객체를 비교하기 위해 Comparable 또는 Comparator을 쓴다는 것은 곧 사용자가 정의한 기준을 토대로 비교를 하여 양수, 0, 음수 중 하나가 반환된다는 것이다.

여기서 정렬과의 관계를 알아보기 전에 Java의 일반적인 정렬기준에 대해 알고가야 할 필요가 있다.

대부분의 언어도 마찬가지지만, **Java에서의 정렬은 특별한 정의가 되어있지 않는 한 '오름차순'을 기준**으로 한다.

우리가 흔히 쓰는 Arrays.sort(), Collections.sort() 모두 오름차순을 기준으로 정렬이 된다는 것이다.

오름차순으로 정렬이 된다는 것은 무엇일까?

예로들어 {1, 3, 2} 배열이 있다고 가정해보자. 그럼 우리가 최종적으로 얻어야 할 배열 {1, 2, 3} 을 얻기 위해 정렬 알고리즘을 사용하게 될 것이다. 이 때, 정렬을 하기 위해 두 원소를 비교 하게 될 것 아닌가? 정렬 메소드에서 두 수를 비교하기 위해 index 0 원소와 index 1 원소를 비교한다고 가정해보자.

그럼 선행 원소인 1과 후행 원소인 3의 경우 대소관계는 어떻게 되는가? 1이 3보다 작다. 

앞서 선행 원소와 후행 원소를 비교 할 때, 얼마큼 차이가 나는지를 반환한다고 헀다.

return o1 - o2; 를 한다면, 1-3 = -2로 '음수'가 나올 것이다. 

이 때, 자바에서는 오름차순을 디폴트 기준으로 삼고 있다고 했다. 이 말은 **선행 원소가 후행 원소보다 '작다'는 뜻**이다.

즉, compare 혹은 compareTo를 사용하여 객체를 비교 할 경우 **음수가 나오면 두 원소의 위치를 바꾸지 않는다는 것**이다.

그 다음 정렬 알고리즘에 의해 index 1 원소와 index 2 원소를 비교한다고 해보자.

선행 원소인 3이 2보다 크다.

compare 혹은 compareTo를 사용하여 index 1 원소와 index 2 원소를 비교한다면 **'양수'**가 나올 것이다. (3-2 = 1) 이는 곧 이러면 선행 원소가 후행 원소보다 크다는 뜻이라는 것이다.

즉, compare 혹은 compareTo를 사용하여 객체를 비교 할 경우 **양수가 나오면 두 원소의 위치를 바꾼다는 것**이다.

그러면 {1, 2, 3} 으로 오름차순으로 정렬 될 것이다.

그럼 규칙을 일반화 할 수 있다.

**[두 수의 비교 결과에 따른 작동 방식]**

**음수**일 경우 : 두 원소의 위치를 **교환 안함**

**양수**일 경우 : 두 원소의 위치를 **교환 함**

### Array.sort에 Comparable 이용

```java
import java.util.Arrays;
 
public class Test {
   
   public static void main(String[] args) {
      
      MyInteger[] arr = new MyInteger[10];
      
      // 객체 배열 초기화 (랜덤 값으로) 
      for(int i = 0; i < 10; i++) {
         arr[i] = new MyInteger((int)(Math.random() * 100));
      }
 
      // 정렬 이전
      System.out.print("정렬 전 : ");
      for(int i = 0; i < 10; i++) {
         System.out.print(arr[i].value + " ");
      }
      System.out.println();
      
      Arrays.sort(arr);
        
      // 정렬 이후
      System.out.print("정렬 후 : ");
      for(int i = 0; i < 10; i++) {
         System.out.print(arr[i].value + " ");
      }
      System.out.println();
   }
   
}
 
class MyInteger implements Comparable<MyInteger> {
   int value;
   
   public MyInteger(int value) {
      this.value = value;
   }
   
   @Override
   public int compareTo(MyInteger o) {
      return this.value - o.value;
   }
   
}
```

### Array.sort에 Comparator 이용

```java
import java.util.Arrays;
import java.util.Comparator;
 
public class Test {
   
   public static void main(String[] args) {
      
      MyInteger[] arr = new MyInteger[10];
      
      // 객체 배열 초기화 (랜덤 값으로) 
      for(int i = 0; i < 10; i++) {
         arr[i] = new MyInteger((int)(Math.random() * 100));
      }
 
      // 정렬 이전
      System.out.print("정렬 전 : ");
      for(int i = 0; i < 10; i++) {
         System.out.print(arr[i].value + " ");
      }
      System.out.println();
      
      Arrays.sort(arr, comp);      // MyInteger에 대한 Comparator을 구현한 익명객체를 넘겨줌
        
      // 정렬 이후
      System.out.print("정렬 후 : ");
      for(int i = 0; i < 10; i++) {
         System.out.print(arr[i].value + " ");
      }
      System.out.println();
   }
 
   
   static Comparator<MyInteger> comp = new Comparator<MyInteger>() {
      
      @Override
      public int compare(MyInteger o1, MyInteger o2) {
         return o1.value - o2.value;
      }
   };
}
 
 
class MyInteger {
   int value;
   
   public MyInteger(int value) {
      this.value = value;
   }
   
}
```

### 오름차순 내림차순 변경

우리가 그동안 compare, compareTo 메소드를 구현할 때 다음과 같이 했다.

```java
// Comparable
public int compareTo(MyClass o) {
  return this.value - o.value;
}
 
// Comparator
public int compare(Myclass o1, MyClass o2) {
  return o1.value - o2.value;
}
```

이는 선행 원소가 후행 원소보다 작으면 compare 혹은 compareTo 메소드의 반환값이 음수가 나오고, 정렬 알고리즘에서는 두 원소를 비교할 때 두 원소는 **오름차순 상태**라는 의미이므로 두 원소가 교환되지 않는다는 것이다.

반대로 선행 원소가 후행원소보다 크면  compare 혹은 compareTo 메소드의 반환값이 양수가 나오고, 정렬 알고리즘에서는 두 원소를 비교할 때 두 원소는 **내림차순 상태**라는 의미이므로 두 원소가 교환된다는 것이다.

즉, 정렬 알고리즘에서는 두 원소를 compare 혹은 compareTo 를 써서 양수값이 나오냐, 음수값이 나오냐에 따라 판단을 한다는 것이다.

위 방법이 오름차순이라면 내림차순으로 정렬하고 싶은 경우 두 원소를 비교한 **반환값을 반대로** 해주면 되는 것 아닌가?

쉽게 말해 두 값의 차가 양수가 된다면 이를 음수로 바꿔 반환해주고, 만약 음수가 된다면 그 값을 양수로 바꾸어 반환해주면 된다는 것이다.

```java
// Comparable
public int compareTo(MyClass o) {
  return o.value - this.value;  // == -(this.value - o.value);
}
 
// Comparator
public int compare(Myclass o1, MyClass o2) {
  return o2.value - o1.value; // == -(o1.value - o2.value);
}
```

#### 원문링크

[자바 [JAVA] - Comparable 과 Comparator의 이해][link1]

[link1]: https://st-lab.tistory.com/243 "link1"