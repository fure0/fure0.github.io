---
layout: post
categories: "DB"
title: "DB-INDEX"
author: "TY_K"
---

## DB-인덱스(INDEX)란？

책의 색인, 목차와 같다.
책에도 목차를 보고 우리가 원하는 페이지에서 내용을 찾아가듯, DB에서도 인덱스가 같은 역할을 수행

### DB인덱스와 데이터의 자료구조

- 인덱스의 자료구조 : Sorted List

> Sorted List는 항상 저장된 값을 정렬하여 유지하는 자료구조

- 데이터의 자료구조 : ArrayList

> Array List는 저장되는 순서에 따라, 정렬없이 값을 저장하는 자료구조

### 인덱스의 자료구조 Sorted List의 장점과 단점

#### 장점
데이터가 이미 정렬되어 있기 때문에, DB에서 SELECT할 경우 속도가 빠르다.

#### 단점
데이터가 변할경우 (INSERT, UPDATE, DELETE) 에는 다시 정렬을 해야하기 때문에 그에 따른 비용이 발생한다.

*즉 읽기 성능을 향상시키기 위해 삽입, 갱신, 삭제의 성능을 낮추는 것. 그렇기 때문에 하나의 테이블에 너무 많은 인덱스를 설정하면 저장성능이 떨어진다.

### 인덱스의 역할

1.Primary Key

- 테이블에서 하나의 레코드를 대표하는 컬럼 값

- 하나의 레코드로 식별할 수 있는 식별자.

- NULL은 허용되지 않음.

- 중복을 허용하지 않음.

2.Secondary Key

- Primary Key를 제외한 나머지 인덱스

- 보조 인덱스 키라고보 불림.

### 인덱스의 데이터 저장 방식

1.B-Tree Index

- 가장 일반적으로 사용되는 인덱스 알고리즘.

- 오래되어서 인증된, 성숙한 상태의 저장방식

- 컬럼값을 변형하지 않고, 원래 값을 기준으로 인덱싱하는 알고리즘

2.Hash Index

- 컬럼값을 해시값으로 계산하여 인덱싱 하는 알고리즘.

- 빠른 속도를 지원

- 해시값으로 변형되어 인덱싱 하기 때문에, 컬럼값 일부분만 검색은 불가능

- 주로 메모리 기반 데이터베이스에서 많이 사용, 이유는 메모리 주소형태로 그 값을 모두 참조하는 형태로 사용하기 때문.

3.Fractal-Tree Index

- B-Tree Index의 단점을 보완하기 위해 고안된 알고리즘

- B-Tree처럼 컬럼값을 변형하지 않고 인덱싱

- 데이터가 저장되거나 삭제될때, 처리하는 비용을 줄일 수 있게 설계되어서 B-Tree보다 경제적

다만 아직 성숙되지 않은 알고리즘

### 왜 index 를 생성하는데 b-tree 를 사용하는가?

데이터에 접근하는 시간복잡도가 O(1)인 hash table 이 더 효율적일 것 같은데? SELECT 질의의 조건에는 부등호(<>) 연산도 포함이 된다. hash table 을 사용하게 된다면 등호(=) 연산이 아닌 부등호 연산의 경우에 문제가 발생한다. 동등 연산(=)에 특화된 hashtable은 데이터베이스의 자료구조로 적합하지 않다.

### 어느컬럼을 인덱스로 지정할 것인가? (Composite Index)

인덱스로 설정하는 필드의 속성이 중요하다. title, author 이 순서로 인덱스를 설정한다면 title 을 search 하는 경우, index 를 생성한 효과를 볼 수 있지만, author 만으로 search 하는 경우, index 를 생성한 것이 소용이 없어진다. 따라서 SELECT 질의를 어떻게 할 것인가가 인덱스를 어떻게 생성할 것인가에 대해 많은 영향을 끼치게 된다

### 인덱스의 성능과 고려사항1 - 많이 생성하면 좋은가?

SELECT 성능을 올리는 인덱스는 항상 좋은건 아니다. INSERT, DELETE, UPDATE의 경우 추가 비용이 발생한다.
INSERT의 경우 인덱스에 대한 데이터도 추가해야 하기 때문에 추가 비용이 발생하고, DELETE의 경우 인덱스에 존재하는 값을 사용하지 않는다는 표시로 남게 된다. 즉 row의 수가 그대로. 이작업을 반복하면 성능에 악영향을 끼친다.

### 인덱스의 성능과 고려사항2 - 어느 데이터 타입에 적합한가?

데이터 형식에 따라 인덱스가 악영향을 미칠수도 있다.
이름, 나이, 성별 세가지의 컬럼의 경우 이름은 온갖 경우의 수가 존재하고, 나이는 INT 타입을 갖고, 성별은 남녀 두 가지 경우에 대해서만 데이터가 존재 할 것이다. 이경우 어느쪽이 효율적일까? 결론은 이름에 대해서만 효율적이다.

왜 성별이나 나이는 인덱스를 생성하면 비효율적일까? 10000 레코드에 해당하는 테이블에 대해서 2000 단위로 성별에 인덱스를 생성했다고 가정하자. 값의 range 가 적은 성별은 인덱스를 읽고 다시 한 번 디스크 I/O 가 발생하기 때문에 그 만큼 비효율적인 것이다.

### B-Tree (참고)
*참고로만

전산학에서 B-트리(B-tree)는 데이터베이스와 파일 시스템에서 널리 사용되는 트리 자료구조의 일종으로, 이진 트리를 확장해 하나의 노드가 가질 수 있는 자식 노드의 최대 숫자가 2보다 큰 트리 구조이다.

방대한 양의 저장된 자료를 검색해야 하는 경우 검색어와 자료를 일일이 비교하는 방식은 비효율적이다. B-트리는 자료를 정렬된 상태로 보관하고, 삽입 및 삭제를 대수 시간으로 할 수 있다. 대부분의 이진 트리는 항목이 삽입될 때 하향식으로 구성되는 데 반해, B-트리는 일반적으로 상향식으로 구성된다.

[참고링크1][link1]

[참고링크2][link2]

[link1]: https://interconnection.tistory.com/97 "참조1"
[link2]: https://github.com/fure0/Interview_Question_for_Beginner/tree/master/Database#index "참조2"