---
layout: post
categories: "Java"
title: "Stream"
author: "TY_K"
---

### 스트림이란

스트림은 컬렉션에 저장된 요소를 하나식 꺼내서 람다식으로 처리하는 반복자이다. 스트림을 사용하기 위해서는 람다식에 대한 지식이 필요하며 컬렉션, 스레드에 대한 이해도 필요하다. java 에서 반복자 대표주자는 Iterator(java.util) 이다. 스트림은 java.util.stream 패키지에 속한 인터페이스이다.

스트림과 컬렉션은 집합에 대한 클래스이다. 컬렉션이 요소를 할당하고 관리하는데 목적이 있다면, 스트림은 요소를 검색하거나 값을 처리하는데 목적이 있다.

### 스트림의 특징

- 람다식으로 처리 -> 코드가 간결해짐
- 내부 반복자 사용 -> 병렬처리가 쉽다
- 중간처리 및 최종처리(집계) 결과 관리가 쉽다

### 참고1) 외부반복자 vs 내부반복자

#### 외부반복자(external iterator)

- 컬렉션의 요소를 하나씩 가져오는 코드를 직접 작성하기
- 외부반복자 예시 : for문, iterator를 사용하는 while문
- 코딩 : 컬렉션 요소 꺼내는 코드 및 요소를 처리하는 코드 작성

#### 내부반복자(internal iterator)

- 컬렉션 내부에서 코드를 하나씩 꺼내줌
- 코딩 : 컬렉션 요소를 처리하는 코드만 작성

### 참고2) 병렬처리란

병렬처리란 한 가지 작업을 여러 스레드가 처리할 수 있도록 여러 작업단위를 나누고 병렬로 처리하는 것을 말한다. 각 스레드가 처리한 결과는 자동으로 결합해 최종수행결과물이 나온다. 작업단위로 나눈 부분의 결과가 중간처리결과, 중간처리결과를 합하면 최종처리결과가 나온다.

### 참고3) 중간처리 & 최종처리란

- 중간처리  : 매핑, 필터링, 정렬 등 작업
- 최종처리 : 요소반복, 개수세기(count), 평균(average), 합계(sum) 등 집계

### 1. 스트림 얻기

컬렉션에서 Iterator 반복자를 얻어내듯이 배열, 컬렉션 등 집합에서 Stream을 얻어낼 수 있다.

- java.util.Arrays - stream() 메서드
- java.util.Collection - stream() 메서드

#### 1-1) 배열에서 스트림 얻기
```java
public class Test {
    public static void main(String[] args) {
        String[] strArr = {"백지영", "박효신", "양파", "버즈"};
        //Arrays 클래스 - stream()메서드로 스트림 얻기
        Stream<String> strStream = Arrays.stream(strArr);
        //forEach는 반복하면서 모든 개별요소를 가져올 수 있다
        strStream.forEach( a -> {
            System.out.print(a+ " ");
        });
        System.out.println();

        int[] intArr = {12, 64, 92};
        //요소가 int 인 경우 IntStream으로 얻는다
        IntStream intStream = Arrays.stream(intArr);
        IntConsumer intConsumer = i -> System.out.print(i+ " ");
        intStream.forEach(intConsumer);
        System.out.println();

        double[] doubleArr = {145.46, 2.85};
        //요소가 double 인 경우 DoubleStream으로 얻는다
        DoubleStream doubleStream = Arrays.stream(doubleArr);
        doubleStream.forEach( a -> System.out.print(a+ " ") );
    }
}

/*
실행 결과
백지영 박효신 양파 버즈 
12 64 92 
145.46 2.85 
*/
```

### 1-2) 컬렉션에서 스트림 얻기

내부클래스 Student 를 요소로 하는 List, Set에서 스트림 얻기

```java
public class Test {
    class Student{
        private int stuNum;
        private String stuName;
        private boolean isCome;

        Student(int stuNum, String stuName, boolean isCome){
            this.stuNum = stuNum;
            this.stuName = stuName;
            this.isCome = isCome;
        }

        @Override
        public String toString() {
            return "Student{" +
                    "stuNum=" + stuNum +
                    ", stuName='" + stuName + '\'' +
                    ", isCome=" + isCome +
                    '}';
        }
    }

    public static void main(String[] args) {
        Test t = new Test();

        Student stu1 = t.new Student(1, "조성모", true);
        Student stu2 = t.new Student(2, "이승기", false);
        Student stu3 = t.new Student(3, "윤도현", false);

        //List 에 요소 추가하고 스트림 얻기
        List<Student> list = new ArrayList<>();
        list.add(stu1);
        list.add(stu2);
        list.add(stu3);

        Stream<Student> stuStream = list.stream();
        stuStream.forEach( a -> System.out.println(a.toString()) );

        //Set 에 요소 추가하고 스트림 얻기
        Set<Student> set = new HashSet<>();
        set.add(stu1);
        set.add(stu2);
        set.add(stu3);

        set.stream()
                .forEach(a -> System.out.println(a));
    }
}
/*
실행 결과
Student{stuNum=1, stuName='조성모', isCome=true}
Student{stuNum=2, stuName='이승기', isCome=false}
Student{stuNum=3, stuName='윤도현', isCome=false}
Student{stuNum=1, stuName='조성모', isCome=true}
Student{stuNum=3, stuName='윤도현', isCome=false}
Student{stuNum=2, stuName='이승기', isCome=false}
*/
```

### 1-3) 숫자범위에서 스트림 얻기

IntSteam의 range() 메서드를 이용해 범위를 지정해서 스트림을 얻을 수 있다.

```java
public class Test {
    public static void main(String[] args) {
        //마지막 숫자 포함
        IntStream intStream1 = IntStream.rangeClosed(1, 10);
        intStream1.forEach( a -> System.out.print(a+ " "));
        System.out.println();

        //마지막 숫자 제외
        IntStream intStream2 = IntStream.range(1, 10);
        intStream2.forEach( a -> System.out.print(a+ " "));
    }
}
/*
실행 결과
1 2 3 4 5 6 7 8 9 10 
1 2 3 4 5 6 7 8 9 
*/
```
### 2. 파이프라인

파이프라인(pipeline)은 데이터의 처리결과가 다시 다음 단계의 입력값으로 들어가 연속적으로 처리되는 구조를 말한다. 수도관이 길게 연결되어 취수, 정수 등의 과정을 거쳐 개별 가정으로 보급되듯, 파이프라인이란 데이터가 일련의 과정을 거쳐 사용자가 원하는 결과값을 내놓도록 처리되는 구조이다.

파이프라인은 여러개의 스트림을 연결하는데 중간처리 스트림 여러개와 최종처리 스트림1개로 구성된다. 필요에 따라 중간처리 스트림을 부품처럼 연결해 사용한다. 중간처리 스트림은 필요한 데이터를 검색하거나 원하는 형태변환하는 등의 처리를 한다. 최종처리 스트림은 중간처리 스트림에서 반환하는 값을 토대로 결과값을 내놓는 처리를 한다.

- 중간처리(intermediate operators) : 필터링, 매핑, 정렬, 그룹핑 등
- 최종처리(terminal operators) : 합계, 평균, 개수, 최댓값, 최솟값 등

대량의 데이터 중에서 우리가 관심있는 데이터만 조건에 걸어 데이터 량을 줄이고 계산(가공)하는 과정을 **리덕션(reduction)**이라고 한다.

### 파이프라인 맛보기

스트림을 연결해 파이프라인 구조를 만들어서 데이터를 처리해보자. 맛보기이므로 '이런식으로 처리되는구나' 하는 느낌만 느껴보자.

Member 클래스를 요소로 하는 List가 있다. 이를 가공해서 남성 나이의 평균값을 구해보자.

먼저 Member 클래스이다.

```java
public class Member {
    public static int MALE = 0;
    public static int FEMALE = 1;

    private String name;
    private int sex;
    private int age;

    Member(String name, int sex, int age){
        this.name = name;
        this.sex = sex;
        this.age = age;
    }

    public int getSex(){ return sex; };
    public int getAge(){ return age; };

    @Override
    public String toString() {
        return "Member{" +
                "name='" + name + '\'' +
                ", sex=" + sex +
                ", age=" + age +
                '}';
    }
}
```

List를 생성해 데이터를 넣고 가공해보기
```java
public class Test {
    public static void main(String[] args) {
        List<Member> list = Arrays.asList(
                new Member("홍길동", Member.MALE, 30),
                new Member("최나리", Member.FEMALE, 26),
                new Member("이태진", Member.FEMALE, 92),
                new Member("최성혁", Member.MALE, 59)
        );

        //f. 변수에 대입
        double averageAge =
                        //a. 원본스트림 생성
                        list.stream()

                        //b. 새로운 스트림 생성--> Male 과 일치하는 값으로 필터링
                        .filter(m -> m.getSex() == Member.MALE)

                        //c. 새로운 스트림 생성--> Member 와 age 매핑하기
                        .mapToInt(Member :: getAge)

                        //d. 평균값을 계산해서 OptionalDouble 객체에 저장
                        .average()

                        //e. 객체에 저장된 값 읽어들이기
                        .getAsDouble();

        System.out.println("남자 평균 나이 : "+ averageAge);


        /* 위 코드 한줄씩 실행해보기 */
        //필터링
        Stream stream1 = list.stream()
                             .filter(m -> m.getSex() == Member.MALE);
        stream1.forEach( a -> System.out.println(a));

        //필터링 + 매핑
        IntStream stream2 = list.stream()
                                .filter(m -> m.getSex() == Member.MALE)
                                .mapToInt(Member::getAge);
        stream2.forEach(a -> System.out.println(a));

        //필터링 + 매핑 + 평균(OptionalDouble로 받기)
        OptionalDouble optionalDouble = list.stream()
                .filter(m -> m.getSex() == Member.MALE)
                .mapToInt(Member::getAge)
                .average();
        System.out.println(optionalDouble);

        //OptionalDouble에 있는 값 얻기
        Double value = optionalDouble.getAsDouble();
        System.out.println(value);
    }
}
/*
실행 결과
남자 평균 나이 : 44.5
Member{name='홍길동', sex=0, age=30}
Member{name='최성혁', sex=0, age=59}
30
59
OptionalDouble[44.5]
44.5
*/
```

뭔가 코드가 길다. 여기서는 스트림을 연결해 내가 원하는 값을 쉽게 구할 수 있다는 점을 느끼면 된다. 아래에서 하나씩 익혀보자.

### 2-1) 필터링 - 요소 걸러내기

- distinct() : 중복 제거하기
- filter() : 조건에 맞는값 찾기

```java
public class Test {
    public static void main(String[] args) {
        List<String> list = Arrays.asList("사과", "사과", "딸기", "바나나", "복숭아", "수박", "딸기", "바나나", "수박", "토마토");

        System.out.println("--중복제거");
        Stream<String> stream = list.stream().distinct();
        stream.forEach(a -> System.out.print(a+ " "));

        System.out.println("\n--이름이 3글자");
        list.stream()
            .filter(fruit -> fruit.length() == 3)
            .forEach(a -> System.out.print(a+ " "));

        System.out.println("\n--이름이 3글자, 중복제거");
        list.stream()
                .distinct()
                .filter(fruit -> fruit.length() == 3)
                .forEach(a -> System.out.print(a+ " "));

        System.out.println("\n--이름이 3글자, 중복제거, 첫글자가 '바'");
        list.stream()
                .distinct()
                .filter(fruit -> fruit.length() == 3)
                .filter(fruit -> fruit.startsWith("바"))
                .forEach(a -> System.out.print(a+ " "));
    }
}
/*
실행 결과
--중복제거
사과 딸기 바나나 복숭아 수박 토마토 
--이름이 3글자
바나나 복숭아 바나나 토마토 
--이름이 3글자, 중복제거
바나나 복숭아 토마토 
--이름이 3글자, 중복제거, 첫글자가 '바'
바나나 
*/
```

### 2-2) 매핑 - 다른 값으로 변경하기

- flatMapXXX() : 특정요소가 여러개의 요소로 대체되는 스트림 리턴
- mapXXX() : 요소를 대체하는 새로운 스트림 리턴
- asXXXStream() : 타입변환된 스트림 리턴
- boxed() : 박싱(boxing)된 값을 요소로 하는 스트림 리턴

flatMap 사용하기
```java
public class Test01 {
    public static void main(String[] args) {
        
        /*
        String 요소를 공백기준으로 각각의 String 요소로 나누기        
        */
        List<String> listString = Arrays.asList(
               "홍 길 동", "임 꺽 정"
        );
        listString.stream()
                .flatMap(a -> Arrays.stream(a.split(" ")))
                .forEach(a -> System.out.print(a+ " "));
        System.out.println();
        
        /*
        String 요소 , 기준으로 분리하기
        각각을 int 형태로 바꾸기        
        */
        List<String> listInt = Arrays.asList(
            "17, 32, 71, 96, 26, 53"
        );
        listInt.stream()
                .flatMapToInt(data -> {
                   String[] strArr = data.split(",");
                   int[] intArr = new int[strArr.length];
                   for(int i=0; i<strArr.length; i++){
                       intArr[i] = Integer.parseInt(strArr[i].trim());
                   }
                   return Arrays.stream(intArr);
                })
                .forEach(a -> System.out.print(a+ " "));
    }
}
/*
실행 결과
홍 길 동 임 꺽 정 
17 32 71 96 26 53 
*/
```

Map 사용하기
```java
public class Test02 {
    public static void main(String[] args) {
        Test02 test = new Test02();
        List<Student> list = Arrays.asList(
            test.new Student("홍길동", 91),
            test.new Student("이민형", 64),
            test.new Student("김기준", 72)
        );

        list.stream()
            .mapToInt(Student :: getScore)
            .forEach(a -> System.out.println("점수 : "+ a));
    }

    class Student{
        private String name;
        private int score;

        private Student(String name, int score){
            this.name = name;
            this.score = score;
        }

        public String getName() {
            return name;
        }
        public int getScore() {
            return score;
        }
    }
}
/*
실행 결과
점수 : 91
점수 : 64
점수 : 72
*/
```

asXXX 사용하기
```java
public class Test03 {
    public static void main(String[] args) {
        int[] intArr = {16, 52, 49, 26, 83};
        IntStream intStream;

        System.out.println("--double 로 변환해서 출력하기");
        intStream = Arrays.stream(intArr);
        intStream.asDoubleStream()
                .forEach( a -> System.out.print(a+ " "));

        System.out.println("\n--boxing 된 값 출력하기");
        intStream = Arrays.stream(intArr);
        intStream.boxed()
                .forEach(a -> System.out.print(a+ " "));
    }
}
/*
실행 결과
--double 로 변환해서 출력하기
16.0 52.0 49.0 26.0 83.0 
--boxing 된 값 출력하기
16 52 49 26 83 
*/
```

### 2-3) 정렬

숫자는 오름차순, 내림차순으로 정렬할 수 있다. 혹은 Comparator를 구현해 정렬기준을 제시할 수 있다.

- sorted() : 오름차순 정렬
- sorted() : Comparable 구현기준에 따라 정렬
- sorted(Comparator<T>) : Comparator 구현기준에 따라 정렬

```java
public class Test {
    public static void main(String[] args) {
        Test t = new Test();
        IntStream intStream = Arrays.stream(new int[]{2, 64, 37, 19, 82});
        System.out.println("--int[] 오름차순 정렬하기");
        intStream.sorted()
                .forEach(a -> System.out.print(a+ " "));

        System.out.println();
        List<Student> list = Arrays.asList(
            t.new Student("홍길동", 35),
            t.new Student("최길동", 26),
            t.new Student("허길동", 15),
            t.new Student("박길동", 96),
            t.new Student("김길동", 68)
        );

        System.out.println("--List - Student 기준에 따라 정렬하기");
        list.stream()
            .sorted()
            .forEach(a-> System.out.println( "이름: "+ a.getName()+ " | 점수: "+  a.getScore()) );

        System.out.println("--List - Student 기준 반대로 정렬하기");
        list.stream()
            .sorted(Comparator.reverseOrder())
            .forEach(a-> System.out.println( "이름: "+ a.getName()+ " | 점수: "+  a.getScore()) );
    }

    //Comparable 구현객체
    class Student implements Comparable<Student>{
        private String name;
        private int score;

        private Student(String name, int score){
            this.name = name;
            this.score = score;
        }

        public String getName() {
            return name;
        }
        public int getScore() {
            return score;
        }

        //compareTo 오버라이드 해서 정렬기준(=점수) 제시
        @Override
        public int compareTo(Student o) {
            return Integer.compare(score, o.score);
        }
    }
}
/*
실행 결과
--int[] 오름차순 정렬하기
2 19 37 64 82 
--List - Student 기준에 따라 정렬하기
이름: 허길동 | 점수: 15
이름: 최길동 | 점수: 26
이름: 홍길동 | 점수: 35
이름: 김길동 | 점수: 68
이름: 박길동 | 점수: 96
--List - Student 기준 반대로 정렬하기
이름: 박길동 | 점수: 96
이름: 김길동 | 점수: 68
이름: 홍길동 | 점수: 35
이름: 최길동 | 점수: 26
이름: 허길동 | 점수: 15
*/
```

### 2-4) 루핑 - 요소 반복하기

#### peek()
- 중간처리 메서드
- 중간에 호출해야 스트림이 동작한다
- 마지막에 호출하면 스트림이 동작하지 않는다

#### forEach()
- 최종처리 메서드
- 마지막에 호출해야 스트림이 동작한다.
- 이후에 다른 메서드를 호출시 컴파일 에러 발생

```java
public class Test {
    public static void main(String[] args) {
        int[] intArr = {6, 1, 2, 5, 7, 3, 4, 8};

        System.out.println("--peek 호출(스트림 동작안함)");
        Arrays.stream(intArr)
             .filter( a-> a%2 == 0)         //짝수 필터링
             .peek( a -> System.out.print(a+ " "));

        System.out.println("--peek 호출 + 최종메서드 호출");
        int total = Arrays.stream(intArr)
                        .filter( a-> a%2 == 0)
                        .peek( a -> System.out.print(a+ " "))
                        .sum();
        System.out.println("\ntotal : "+ total);

        System.out.println("--forEach 호출");
        Arrays.stream(intArr)
                .filter( a-> a%2 == 0)
                .forEach(a -> System.out.print(a+ " "));

        System.out.println("\n--forEach + 메서드 호출(컴파일에러)");
//        Arrays.stream(intArr)
//                .filter( a-> a%2 == 0)
//                .forEach(a -> System.out.println(a))
//                .count();
    }
}
/*
실행 결과
--peek 호출(스트림 동작안함)
--peek 호출 + 최종메서드 호출
6 2 4 8 
total : 20
--forEach 호출
6 2 4 8 
--forEach + 메서드 호출(컴파일에러)
*/
```

### 2-5) 매칭

### 기본집계

#### 2-6-1) 합계, 평균, 최댓값, 최솟값, 카운트 등 최종값 구하기

- count() : 개수
- findFirst() : 첫번째 요소
- max() : 최댓값
- min() : 최솟값
- sum() : 합계
- average() : 평균

```java
public class Test01 {
    public static void main(String[] args) {
        int[] intArr = {6, 1, 2, 5, 7, 3, 4, 8};

        long count = Arrays.stream(intArr)
                            .filter( a-> a%2 == 0)
                            .count();
        System.out.println("count : "+ count);

        long sum = Arrays.stream(intArr)
                            .filter( a-> a%2 == 0)
                            .sum();
        System.out.println("sum : "+ sum);

        double average = Arrays.stream(intArr)
                                .filter( a-> a%2 == 0)
                                .average()
                                .getAsDouble();
        System.out.println("average : "+ average);

        int max = Arrays.stream(intArr)
                        .filter( a-> a%2 == 0)
                        .max()
                        .getAsInt();
        System.out.println("max : "+ max);

        int min = Arrays.stream(intArr)
                        .filter( a-> a%2 == 0)
                        .min()
                        .getAsInt();
        System.out.println("min : "+ min);

        int first = Arrays.stream(intArr)
                            .filter( a-> a%2 == 0)
                            .findFirst()
                            .getAsInt();
        System.out.println("first : "+ first);
    }
}
/*
실행 결과
count : 4
sum : 20
average : 5.0
max : 8
min : 2
first : 6
*/
```

#### 2-6-2) Optional 클래스 사용하기

집계된 값을 저장하고, 집계값이 존재하지 않을 경우 대체값 설정할 수 있다

- isPresent() : 값이 저장되었는지 여부
- orElse() : 값이 저장되지 않았다면 대체값 설정
- ifPresent() : 값이 저장된 경우 Consumer 에서 처리하기

```java
public class Test02 {
    public static void main(String[] args) {
        List<Integer> list = new ArrayList<>();
        OptionalDouble optionalDouble = list.stream()
                                            .mapToInt(Integer :: intValue)
                                            .average();
        //값이 존재하는지 확인하고 대체값 설정
        if(optionalDouble.isPresent()){
            System.out.println(optionalDouble.getAsDouble());
        } else {
            System.out.println("avg1 : 0.0");
        }

        //값이 존재하는지 확인하고 대체값 설정
        double avg = list.stream()
                        .mapToInt(Integer :: intValue)
                        .average()
                        .orElse(0.0);
        System.out.println("avg2 : "+ avg);

        //값이 존재하는지 확인하고 있다면 값 출력하기
        list.stream()
            .mapToInt(Integer :: intValue)
            .average()
            .ifPresent(a -> System.out.println("avg3 : "+ a));
    }
}
/*
실행 결과
avg1 : 0.0
avg2 : 0.0
*/
```

### 2-7) 수집

Student 클래스를 요소로 하는 컬렉션에서 조건에 일치하는 요소로 새로운 컬렉션 생성하기
```java
public class Test {
    public enum Sex {MALE, FEMALE}
    public enum City {SEOUL, BUSAN}

    class Student{
        private String name;
        private int score;
        private Sex sex;
        private City city;

        private Student(String name, int score, Sex sex, City city){
            this.name = name;
            this.score = score;
            this.sex = sex;
            this.city = city;
        }

        public String getName() {
            return name;
        }
        public int getScore() {
            return score;
        }
        public Sex getSex() {
            return sex;
        }
        public City getCity() {
            return city;
        }
    }

    public static void main(String[] args) {
        Test t = new Test();
        List<Student> list = Arrays.asList(
                t.new Student("이순신", 25, Sex.MALE, City.BUSAN),
                t.new Student("이순령", 55, Sex.FEMALE, City.SEOUL),
                t.new Student("이순자", 74, Sex.FEMALE, City.BUSAN),
                t.new Student("이순근", 17, Sex.MALE, City.SEOUL),
                t.new Student("이순혜", 35, Sex.FEMALE, City.SEOUL)
        );

        System.out.println("--List -Male");
        List<Student> listMale = list.stream()
                                    .filter(a -> a.getSex() == Sex.MALE)
                                    .collect(Collectors.toList());
        listMale.stream()
                .forEach(a-> System.out.print(a.getName()+ " "));

        System.out.println("\n--Set -부산");
        Set<Student> setFemale = list.stream()
                                    .filter(a -> a.getCity() == City.BUSAN)
                                    .collect(Collectors.toCollection(HashSet::new));
        setFemale.stream()
                 .forEach(a -> System.out.print(a.getName()+ " "));
    }
}
/*
실행 결과
--List -Male
이순신 이순근 
--Set -부산
이순신 이순자 
*/
```

#### 원문링크

[[Java] 스트림(Stream) 익히기][link1]

[link1]: https://makecodework.tistory.com/entry/Java-%EC%8A%A4%ED%8A%B8%EB%A6%BCStream-%EC%9D%B5%ED%9E%88%EA%B8%B0 "link1"