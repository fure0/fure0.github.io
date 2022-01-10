---
layout: post
categories: "DB"
title: "Primary Key And Foreign Key"
author: "TY_K"
---

## Database의 기본키(Primary Key), 외래키(Foreign Key)란?

* PK와 FK는 테이블의 필수 요소로 모든 테이블은 이들 중 하나 이상을 반드시 포함
* DB에서 테이블을 생성할 때 하나 이상의 항목을 기본키(PK)로 설정 가능
* PK = Not null + Unique

## Primary Key
* 다른 항목과 중복되어 나타날 수 없는 단일값(unique)
* 기본키는 null 값을 가질 수 없음
* 하나 이상의 컬럼이 그룹화 되어 기본키로 쓰일 수 있음
* 고유 인덱스가 자동생성 됨
* 테이블은 기본키를 하나까지만 가질 수 있음

  기술적 측면에서 기본키는 단일값(unique)하고 not Null이면 기능적으로 동일하게 작동하지만,
실제적으로 기본키처럼 구분되는건 오직 하나, 즉 다 똑같이 Unique하고 not Null이라고 기본키가 되는것이 아님

## Foreign Key
* 테이블을 생성할 때 FK를 정의
* FK가 정의된 테이블은 자식 테이블
* 참조되는 테이블은 부모 테이블
* 부모 테이블은 미리 생성되어 있어야 한다
* 부모 테이블의 참조되는 컬럼에 존재하는 값만 입력 가능 ( ex:등록된 회원ID만 주문가능)
* 부모 테이블은 FK로 인해 삭제가 불가능

[참고링크][PKFK]

[PKFK]: http://itnovice1.blogspot.com/2019/01/database-primary-key.html "PKFK"