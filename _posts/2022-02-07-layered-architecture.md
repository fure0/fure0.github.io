---
published: true 
layout: post
categories: "DB"
title: "Layered Architecture"
author: "TY_K"
---

<style>
    .post img {
        margin : 0
    }
</style>

### 계층화 아키텍쳐(Layered Architecture)의 구성요소

![image1](https://static.javatpoint.com/springboot/images/spring-boot-architecture.png){: width="60%" height="60%"}
(https://www.javatpoint.com/spring-boot-architecture)

### 1. Presentation layer : View

> 표현과 이벤트 처리, 데이터 포맷 책임

사용자와의 최종 접점에 위치하여 사용자로부터 데이터를 입력 받거나, 요청된 데이터를 출력해 보이는 계층이다. 외부로부터의 어플리케이션에 대한 요구를 받아들이는 동시에 처리 결과를 사용자에게 보여주는 부분이며, 컨트롤러의 구성 요소와 상호작용한다.

### 2. Control layer : Controller

> 구성 요소간의 처리 흐름을 제어, 데이터 조작 의뢰, 데이터 변환 및 연산, Exception과 Error 처리

사용자로부터 요청을 받고 응답을 처리하는 계층이다. 하위 계층에서 발생하는 Exception이나 Error에 대한 처리를 맡으며, 최종 프레젠테션 계층에 표현 해야할 도메인 모델을 엮는 기능을 수행한다. 사용자로부터 전달 받은 데이터의 유효성 검증을 처리하고, 전체 시스템의 설정상태를 유지한다. 요청에 해당하는 비지니스 로직을 결정하는 역할을 수행한다.

사용자의 요청을 검증, 로직에 요청을 전달, 로직에서 반환된 응답을 적절한 뷰로 연결

### 3. Business layer : Service

> Control layer와 Persistence layer를 연결하는 역할, Transaction 처리

애플리케이션의 비지니스 로직 처리와 비지니스와 관련된 도메인 모델의 적합성을 검증하고, 트랜잭션을 처리한다. Control계층과 Persistence계층을 연결하는 역할로서 두 계층이 직접 통신하지 않게 하여 애플리케이션의 유연성을 증가시킨다.

### 4. Persistence layer : DAO

영구 데이터를 빼내어 객체화 시키며, 영구 저장소에 데이터를 저장, 수정, 삭제하는 계층이다. 데이터베이스나 파일에 접근하여 데이터를 CRUD하는 계층이다.

### 5. Domain Model Layer : VO, DTO

관계형 데이터베이스의 엔티티와 비슷한 개념을 가지는 것으로 데이터 객체를 말한다.

6. 정리

위와 같이 계층을 분리하는 이유는 간단하다. 한 곳에서 위의 모든 작업이 한꺼번에 이루어진다면 코드의 복잡성 증가, 유지보수의 어려움과 유연성 부족, 중복 코드의 증가, 낮은 확장성 등의 문제가 발생하기 때문이다.

#### 원문링크

[계층화 아키텍쳐(Layered Architecture)의 구성요소][link1]

[link1]: https://walbatrossw.github.io/etc/2018/02/26/etc-layered-architecture.html "link1"