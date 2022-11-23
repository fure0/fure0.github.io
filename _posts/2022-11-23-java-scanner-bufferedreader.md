---
layout: post
categories: "Java"
title: "Java Scanner and BufferedReader"
author: "TY_K"
---

### Scanner

- java.util.Scanner 클래스
- 데이터 형을 받기 편하다.(문자열로 받는건 같지만 입력하면서 바로 형변환이 일어난다.)
- 입력값의 경계로 공백, 엔터 모두 인식이 가능하다.
- IOException을 숨긴다.
- 동기화 되지 않는다.
- buffer 사이즈 1024

### BufferedReader

- Java.io.BufferedReader 클래스
- 데이터가 문자열로 먼저 저장되기 때문에 형변환 필수
- 입력값이 엔터만 인식하기때문에 한 라인에 여러가지 입력하고 싶으면 stringtokenize 필수
  <br>(stringtokenize없이 입력하면 공백을 문자열로 인식)
- IOException을 던져야 한다.(throws) 
- 입력과 동시에 동기화 된다.
- buffer 사이즈 8192

얼핏보면 bufferedreader가 더 좋아보이지 않는다. 하지만 이 두 방식의 큰 차이는 속도의 차이가 있는데 이 차이가 장 단점을 다 뒤집을 만큼의 큰 차이다.

### 수행시간

- Scanner : 6.068(s)
- BufferedReader : 0.934(s)

### 왜 Bufferedreader가 빠른가?

bufferedreader는 기존의 inputstreamreader에 버퍼링이 추가된 class이다.

기존의 inputstreamreader는 문자열을 한글자씩 읽어왔었지만 여기에 buffer를 추가하므로써 문자열을 한번에 저장하고 필요할 때 꺼내 올 수 있게 되었다.

읽을면서 출력해야하는 것보단 일단 저장하고 필요할때 출력만 하면 되기 때문에 속도가 빨라지고 부하가 적다.

### Scanner 코드
```java
import java.util.Scanner;

public class scanner {
  
  //IOException을 던질 필요 없음
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);
    //형 변환을 따로 안해줘도 알아서 int형으로 저장된다.
    //변수 형태를 double로하고 싶으면 sc.nextDouble();로 하면 된다.
    int N = sc.nextInt();

    System.out.println(N);
  }

}
```

### BufferedReader 코드
```java
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.BufferedReader;

public class bufferedreader {

  //IOException을 던져야함
  public static void main(String[] args) throws IOException{
    //bufferedreader는 설명에서처럼 inputstreamreader에 buffer를 추가하는 것이기 떄문에 
    //inputstreamreader를 받아와야한다.
    BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    //bufferedreader는 무조건 우선 문자열로 저장하기 때문에 다른 형으로 저장하고 싶으면 형 변환을 해줘야한다.
    int N = Integer.parseInt(br.readLine());
    //문자열로 저장할거면 형변환 없이 그냥 저장하면 된다.
    String S = br.readLine();
    
    System.out.println(N);
    System.out.println(S);
  }

}
```

#### 원문링크

[(JAVA / 자바) Scanner 와 Bufferedreader][link1]

[link1]: https://comain.tistory.com/3 "link1"

