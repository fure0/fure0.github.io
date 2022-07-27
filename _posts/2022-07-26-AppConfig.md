---
layout: post
categories: "Spring"
title: "AppConfig"
author: "TY_K"
---

이 게시글은 개인 공부용으로 AppConfig의 발전 단계를 코드별로 정리한 것입니다.

김영한님의 "스프링 핵심 원리 - 기본편" 의 내용을 참조한 것입니다.

변수 및 메소드명을 변경하였습니다.

[스프링 핵심 원리 - 기본편][link1]

[link1]: https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8/dashboard "link1"

### AppConfig 이전

#### KokApp
```java
public static void main(String[] args) {

    KokService kokService = new KokServiceImpl();
}
```

#### JumunApp
```java
public static void main(String[] args) {

    KokService kokService = new KokServiceImpl();
    JumunService jumunService = new JumunServiceImpl();

}
```

### AppConfig 이후
```java
public class AppConfig {

    public KokService kokService() {
        return new KokServiceImpl(new MemoryKokRepository());
    }

    public JumunService jumunService() {
        return new JumunServiceImpl(
            new MemoryKokRepository(),
            new FixWaribikiPolicy()
        );
    }

}
```

#### KokServiceImpl - 생성자 주입
```java
public class KokServiceImpl implements KokService {

    private final KokRepository kokRepository;

    public KokServiceImpl(KokRepository kokRepository) {
        this.kokRepository = kokRepository;
    }
}
```

#### JumunServiceImpl - 생성자 주입
```java
public class JumunServiceImpl implements JumunService {
    
    private final KokRepository kokRepository;
    private final WaribikiPolicy waribikiPolicy;

    public JumunServiceImpl(KokRepository kokRepository, WaribikiPolicy waribikiPolicy) {
        this.kokRepository = kokRepository;
        this.waribikiPolicy = waribikiPolicy;
    }

}
```

### AppConfig 실행

#### 사용 클래스 - KokApp
```java
public class KokApp {

    public static void main(String[] args) {

        AppConfig appConfig = new AppConfig();
        KokService kokService = appConfig.kokService();

    }

}
```

#### 사용 클래스 - JumunApp
```java
public class JumunApp {

    public static void main(String[] args) {

        AppConfig appConfig = new AppConfig();
        KokService kokService = appConfig.kokService();
        JumunService jumunService = appConfig.jumunService();

    }

}
```

### AppConfig 리팩터링

#### 전
```java
public class AppConfig {

    public KokService kokService() {
        return new KokServiceImpl(new MemoryKokRepository());
    }

    public JumunService jumunService() {
        return new JumunServiceImpl(
            new MemoryKokRepository(),
            new FixWaribikiPolicy()
        );
    }

}
```
#### 후
```java
public class AppConfig {

    public KokService kokService() {
        return new KokServiceImpl(kokRepository());
    }

    public JumunService jumunService() {
        return new JumunServiceImpl(kokRepository(), waribikiPolicy());
    }

    public KokRepository kokRepository() {
        return new MemoryKokRepository();
    }

    public WaribikiPolicy waribikiPolicy() {
        return new FixWaribikiPolicy();
    }

}
```

### AppConfig 스프링 기반으로 변경

```java
@Configuration 
public class AppConfig {

    @Bean 
    public KokService kokService() {
        return new KokServiceImpl(kokRepository());
    }

    @Bean 
    public JumunService jumunService() {
        return new JumunServiceImpl(kokRepository(), waribikiPolicy());
    }

    @Bean 
    public KokRepository kokRepository() {
        return new MemoryKokRepository();
    }

    @Bean 
    public WaribikiPolicy waribikiPolicy() {
        return new RateWaribikiPolicy();
    }

}

```

#### KokApp에 스프링 컨테이너 적용
```java
public class KokApp {
    public static void main(String[] args) {
        // AppConfig appConfig = new AppConfig();
        // KokService kokService = appConfig.kokService();
        ApplicationContext applicationContext = 
            new AnnotationConfigApplicationContext(AppConfig.class);

        KokService kokService = 
            applicationContext.getBean("kokService", KokService.class);
    }
}
```

#### JumunApp에 스프링 컨테이너 적용
```java
public class JumunApp {
    
    public static void main(String[] args) {
        // AppConfig appConfig = new AppConfig();
        // KokService kokService = appConfig.kokService();
        // JumunService jumunService = appConfig.jumunService();
        ApplicationContext applicationContext = 
            new AnnotationConfigApplicationContext(AppConfig.class);

        KokService kokService = 
            applicationContext.getBean("kokService", KokService.class);

        JumunService jumunService = 
            applicationContext.getBean("jumunService", JumunService.class);
    }
}
```

### 컴포넌트 스캔과 의존관계 자동 주입

#### AutoAppConfig.java
```java
@Configuration 
@ComponentScan
public class AutoAppConfig {

}
```

#### MemoryKokRepository @Component 추가
```java
@Component
public class MemoryKokRepository implements KokRepository {}
```

#### RateWaribikiPolicy @Component 추가
```java
@Component
public class RateWaribikiPolicy implements WaribikiPolicy {}
```

#### KokServiceImpl @Component, @Autowired 추가
```java
@Component 
public class KokServiceImpl implements KokService {

    private final KokRepository kokRepository;

    @Autowired 
    public KokServiceImpl(KokRepository kokRepository) {
        this.kokRepository = kokRepository;
    }

}

```

#### JumunServiceImpl @Component, @Autowired 추가
```java
@Component 
public class JumunServiceImpl implements JumunService {

    private final KokRepository kokRepository;
    private final WaribikiPolicy waribikiPolicy;

    @Autowired 
    public JumunServiceImpl(KokRepository kokRepository, WaribikiPolicy waribikiPolicy) {
        this.kokRepository = kokRepository;
        this.waribikiPolicy = waribikiPolicy;
    }
}
```

#### AutoAppConfigTest.java
```java
public class AutoAppConfigTest {
    @Test void 
    basicScan() {
        ApplicationContext ac = new AnnotationConfigApplicationContext(AutoAppConfig.class);
        KokService kokService = ac.getBean(KokService.class);
        assertThat(kokService).isInstanceOf(KokService.class);
    }
}
```

### 롬복과 최신 트랜드

#### 기본 코드
```java
@Component 
public class JumunServiceImpl implements JumunService {

    private final KokRepository kokRepository;
    private final WaribikiPolicy waribikiPolicy;

    @Autowired // 생성자가 딱 1개만 있으면 @Autowired 를 생략할 수 있다.
    public JumunServiceImpl(KokRepository kokRepository, WaribikiPolicy waribikiPolicy) {
        this.kokRepository = kokRepository;
        this.waribikiPolicy = waribikiPolicy;
    }

}
```

#### 결과 코드
```java
@Component 
@RequiredArgsConstructor 
public class JumunServiceImpl implements JumunService {

    private final KokRepository kokRepository;
    private final WaribikiPolicy waribikiPolicy;

}
```

롬복 라이브러리가 제공하는 @RequiredArgsConstructor 기능을 사용하면 final이 붙은 필드를 모아서 생성자를 자동으로 만들어준다.