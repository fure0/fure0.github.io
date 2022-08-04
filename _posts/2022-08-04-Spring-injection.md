---
layout: post
categories: "Spring"
title: "Service Repository Injection"
author: "TY_K"
---

김영한님의 "실전! 스프링 부트와 JPA 활용1" 의 내용을 참조한 것입니다.

스프링에서 Service와 Repository에 주입하는 방법

### Service의 경우

#### 필드 주입
```java
@Service
public class MemberService {

    @Autowired
    MemberRepository memberRepository;
    ...
}

```

#### 생성자 주입
```java
@Service
public class MemberService {

    private final MemberRepository memberRepository;

    public MemberService(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }
    ... 
}
```
- 생성자 주입 방식을 권장
- 변경 불가능한 안전한 객체 생성 가능
- 생성자가 하나면, @Autowired 를 생략할 수 있다.
- final 키워드를 추가하면 컴파일 시점에 memberRepository 를 설정하지 않는 오류를 체크할 수 있다. (보통 기본 생성자를 추가할 때 발견)

#### lombok
```java
@Service
@RequiredArgsConstructor
public class MemberService {
    
    private final MemberRepository memberRepository;
    ... 
}
```

### Repository의 경우

```java
import org.springframework.stereotype.Repository;
import javax.persistence.EntityManager;
import javax.persistence.PersistenceContext;

@Repository
public class MemberRepository {

    @PersistenceContext
    private EntityManager em;

    ...

```

스프링 데이터 JPA를 사용하면 EntityManager 도 주입 가능

```java
@Repository
@RequiredArgsConstructor
public class MemberRepository {

    private final EntityManager em;
    ... 
}

```

