---
layout: post
categories: "Spring"
title: "Entity, DTO, VO"
author: "TY_K"
---

<style>
    .post img {
        margin : 0
    }
</style>

Entity, DTO, VO 클래스는 사람마다 사용방법이 조금씩 다릅니다. VO(Value Object)와 DTO(Data Transfer Object)의 사용 방법이 같다고 생각하는 개발자 분들도 많을 것입니다. 

하지만 위 3가지의 클래스들에 대한 특징을 알고 있으면 클래스를 구분지어야 하는 기준점이 생깁니다.

### Entity

- 실제 DB의 테이블과 매칭될 클래스
- 즉, 테이블과 링크될 클래스임을 나타낸다.
- Entity 클래스 또는 가장 Core한 클래스라고 부른다.

Entity 클래스는 DB의 테이블내에 존재하는 컬럼만을 속성(필드)으로 가지는 클래스를 말합니다. 엔티티 클래스는 상속을 받거나 구현체여서는 안되며, 테이블내에 존재하지 않는 컬럼을 가져서도 안됩니다.

예를들어 ARTICLE (게시판) 테이블 내에 TITLE, CONTENT, WRITER를 컬럼으로 가지고 있을 경우, Entity 클래스의 속성도 title, content, writer만 가져야합니다.

JPA를 사용하게 될 경우 엔티티 클래스에는 @Entity 어노테이션을 명시해야 합니다. @Entity는 엔티티 클래스임을 지정하며, 테이블과 1:1로 매핑됩니다.

```java
@Entity
class Article {
    private String title;
    private String contents;
    private String writer;
}
```
### DTO (Data Transfer Object)

- 계층간 데이터 교환을 위한 객체(Java Beans)이다.
- DB에서 데이터를 얻어 Service나 Controller 등으터 보낼 때 사용하는 객체를 말한다.
- 즉, DB의 데이터가 Presentation Logic Tier로 넘어오게 될 때는 DTO의 모습으로 바껴서 오고가는 것이다.
- 로직을 갖고 있지 않는 순수한 데이터 객체이며, getter/setter 메서드만을 갖는다.
- 하지만 DB에서 꺼낸 값을 임의로 변경할 필요가 없기 때문에 DTO클래스에는 setter가 없다. (대신 생성자에서 값을 할당한다.)

DTO(Data Transfer Object)는 데이터 전송(이동) 객체라는 의미를 가지고 있습니다. DTO는 주로 API 데이터를 반환하거나 읽기 모델에 사용 합니다.

예를들어 Article 클래스내에 title, content, writer, regDate, modDate 를 필드로 가지고 있으며, JSON 형식으로 변환해서 보내야할 속성은 title, content, writer라고 가정할때, 해당 속성만을 클래스로 가지는 DTO를 아래처럼 만들면 됩니다.

```java
@Getter @Setter
class ArticleDTO {
  private String title;
  private String content;
  private String writer;
}
```

### VO (Value Object)

- VO는 DTO와 동일한 개념이지만 read only 속성을 갖는다.
- VO는 특정한 비즈니스 값을 담는 객체이고, DTO는 Layer간의 통신 용도로 오고가는 객체를 말한다.

VO(Value Object)는 말 그대로값 객체 라는 의미를 가지고 있습니다. 

VO는 Getter와 Setter를 가질 수 있으며, VO는 테이블 내에 있는 속성 외에 추가적인 속성을 가질 수 있으며, 여러 테이블(A, B, C)에 대한 공통 속성을 모아서 만든 BaseVO 클래스를 상속받아서 사용할 수 도있습니다.

~~VO의 핵심 역할은 equals()와 hashcode() 를 오버라이딩 하는 것입니다. 즉, VO 내부에 선언된 속성(필드)의 모든 값들이 VO 객체마다 값이 같아야, 똑같은 객체라고 판별합니다.~~

```java
@Getter @Setter
@Alias("article")
class ArticleVO {
    private Long id;
    private String title;
    private String contents;

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Article article = (Article) o;
        return Objects.equals(id, article.id);
    }

    @Override
    public int hashCode() {
        return Objects.hash(id);
    }
}
```

![img1](https://gmlwjd9405.github.io/images/spring-framework/spring-package-flow.png){: width="80%" height="80%"}

### JPA 엔티티 작성 - Setter 금지

#### Setter 사용 금지 및 생성자 접근 제어

엔티티를 작성할 때 습관적으로 모든 필드에 Setter를 생성하는 경우가 있습니다.
하지만 Setter를 무분별하게 남용하다 보면 여기저기서 객체(엔티티)의 값을 변경할 수 있으므로 객체의 일관성을 보장할 수 없습니다.

그리고 setter는 그 의도를 알기 힘들다고 생각합니다.

예를 들어 아래의 코드의 경우 멤버 객체를 set매소드를 통해 변경하는데 무엇을 하는건지 한번에 알 수 없습니다.(예제의 경우 간단한 변경이지만 복잡해질 경우 객체의 값을 변경하는 행위가 무엇을 위해 변경하는지 한 눈에 알기 힘들 수 있습니다.)

```java
Member member = new Member();
member.setName("신이삼");
member.setTel("01050503453");
...
member.set...
```

아래의 코드 처럼 객체에서 매소드를 제공하여 변경하면 변경 의도를 한번에 알 수 있고, 객체 자신의 값을 자신이 변경하는 것이 객체 지향 관점에도 더 맞는다고 생각합니다.


```java
// 멤버의 기본정보를 수정한다는 것을 한눈에 알 수 있다
member.changeBasicInfo("홍길동","01012345678");
```
```java
//Member 엔티티 내부에 매서드 생성
public void changeBasicInfo(String name, String tel) {
    this.name = name;
    this.tel = tel;
}
```
객체의 일관성을 유지하기 위해 객체 생성 시점에 값들을 넣어줌으로써 Setter 사용을 줄일 수 있습니다.

```java
// 객체의 생성자 설정 (필드가 많을경우 롬복의 @Builder 사용하면 좋다)
@Builder
public Member(String username, String password, String name, String tel, Address address) {
    this.username = username;
    this.password = password;
    this.name = name;
    this.tel = tel;
    this.address = address;
}
```
```java
// 객체 생성 시 값 세팅(빌더패턴 사용)
Member member = Member.Builder()
      .username("name")
      .password("1234")
      .name("name");
      .tel("01012345677")
      .address(address)
      .build();
```

아래와 같이 기본 생성자 접근자를 protected로 변경하면 new Member() 사용을 막을 수 있어 객체의 일관성을 더 유지할 수 있습니다.
(protected로 설정하는 이유는 JPA 기본 스펙 상 기본 생성자가 필요한데 protected로 제어하는 것 까지 허용되기 때문입니다.)

```java
// Member 엔티티
@Entity
@Getter
@Table(name = "member")
public class Member{

    // 기본 생성자 protected로 접근 제어
    protected Member(){};
    
    ...
}
```

롬복을 사용한다면 @NoArgsconstructor 어노테이션 설정을 통해 간단하게 설정할 수 있습니다.

```java
@Entity
@NoArgsConstructor(access = AccessLevel.PROTECTED)
@Getter
@Table(name = "member")
public class Member {

}
```

#### 원문링크

[Entity, VO, DTO][link1]

[JPA 엔티티 작성 - Setter 금지][link2]

[link1]: https://medium.com/webeveloper/entity-vo-dto-666bc72614bb "link1"

[link2]: https://velog.io/@aidenshin/%EB%82%B4%EA%B0%80-%EC%83%9D%EA%B0%81%ED%95%98%EB%8A%94-JPA-%EC%97%94%ED%8B%B0%ED%8B%B0-%EC%9E%91%EC%84%B1-%EC%9B%90%EC%B9%99 "link2"