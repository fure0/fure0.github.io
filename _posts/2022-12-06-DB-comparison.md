---
published: true 
layout: post
categories: "DB"
title: "DB Comparison"
author: "TY_K"
---

<style>
    li { line-height : 1.8; }
    table {
        border-collapse: collapse;
    }
    table tr td{
        border:1px solid;
        padding:4px;
    }
    tr:nth-child(1) {
      background:black;
      color:white;
    }
</style>

## DBMS 종류

여러가지 RDBMS중에서 Oracle, MS-SQL, MySQL, PostgreSQL 4가지 DBMS에 대해서 특징과 차이점에 대해서 알아보겠습니다.

### 1. Oracle

#### 1.1 정의

Oracle Database는 미국 Oracle 사의 관계형 데이터베이스 관리 시스템입니다.

#### 1.2 특징

1. Oracle은 대용량 데이터 처리와 유지보수의 장점으로 인해 많은 금융권 회사 및 대기업들이 사용을 하였습니다.
2. Oracle의 SQL작성은 Oracle의 SQL문과 ANSI/ISO SQL을 모두 지원합니다.
3. 다양한 DB 통계 정보를 이용한 최적의 쿼리 플랜을 제공합니다.
4. Result Cache 지원<br>
Result Cache는 Oracle 11g 부터 추가된 기능으로, 쿼리의 결과를 메모리에 Cache 하여 같은 쿼리로 요청이 왔을 때 메모리에 있는 결과를 반환하여 성능을 향상하는 기능입니다.

#### 1.3 차이점

- 지원하는 Isolation Level :<br>
  Oracle은 아래 3가지의 Isolation Level을 지원합니다.
  - Read Committed (Default)
  - Serializable
  - Read-Only

- Index Null 저장 여부<br>
  Oracle에서는 Index에 Null을 저장하지 않습니다.

### 2. MS-SQL(SQL Server)

#### 2.1 정의

MS-SQL은 Microsoft에서 개발한 관계형 데이터베이스입니다.

#### 2.2 특징

1. MS의 다른 개발 언어들과 호환성이 뛰어납니다.
2. 초창기 MS-SQL은 window OS에서만 구동이 가능하였습니다.
  하지만 SQL Server 2017 부터는 리눅스에도 설치가 가능합니다.
3. Multi result Set
  MS-SQL은 한번의 SQL 실행으로 여러개의 결과 집합을 받을 수 있습니다.
  예를들어 하나의 쿼리에 여러개의 SELECT 문을 사용하여 여러개의 결과를 한번에 받을 수 있습니다.

#### 2.3 차이점

- 지원하는 Isolation Level :
  MS-SQL은 아래 6가지의 Isolation Level을 지원합니다.
  - Read UnCommitted
  - Read Committed (Default)
  - Repeatable Read
  - Serializable
  - Read Committed Snapshot
  - Snapshot
- Index Null 저장 여부
  MS-SQL에서는 Index에 Null을 저장합니다.

### 3. MySQL

#### 3.1 정의

MySQL은 MYSQL AB에서 개발한 오픈소스 RDBMS입니다.(현재는 Oracle이 인수)

#### 3.2 특징

1. 오픈소스 RDBMS
2. 5.5 버전 이전에는 트랜잭션 지원이 안됐었다.(InnoDB Engine 지원 후 사용 가능, 단 테이블 엔진을 MyISAM으로 설정하면 사용 불가)

#### 3.3 차이점

- 지원하는 Isolation Level :
  특이하게 MySQL은 Default Isolation Level이 Repeatable Read다.
  - Read UnCommitted
  - Read Committed
  - Repeatable Read (Default)
  - Serializable

- Index Null 저장 여부
  MySQL에서는 Index에 Null을 저장합니다.

### 4. PostgreSQL

#### 4.1 정의

PostgreSQL은 객체-관계형 DBMS(ORDBMS)으로, 다양한 DB 객체를 사용자가 만들수 있는 기능을 제공하는 DBMS입니다.

#### 4.2 특징

1. 2021년 9월 버전 14 릴리스부터 PostgreSQL은 SQL:2016 Core 적합성을 위한 179개의 필수 기능 중 최소 170개를 준수합니다.
2. 오픈소스 DBMS지만 다양한 기능들을 제공합니다.

#### 4.3 차이점

- 지원하는 Isolation Level :
  - Read Committed (Default)
  - Repeatable Read
  - Serializable

- Index Null 저장 여부
  PostgreSQL에서는 Index에 Null을 저장하지 않습니다.

- Update 후 정렬순서 변경
  PostgreSQL의 MVCC방식은 update 진행 시 새로운 Version의 row가 생성 되는 방식입니다.
  이를 확인 하기 위하여 CTID를 사용하여 mmbr_id = 'savernet2'를 update해보겠습니다.

> CTID란?<br>
> CTID는 PostgreSQL에서 테이블에 있는 row 버전의 물리적인 위치를 나타냅니다. 하지만 CTID는 VACUUM에 의해서 변경될 수 있습니다.

- update 전 CTID

![image2](https://velog.velcdn.com/images%2Fsavernet%2Fpost%2Feece1cb8-dfda-48b1-abe0-046c73bade03%2Fupdate%20%EC%A0%84.PNG)

- update 전 CTID

![image2](https://velog.velcdn.com/images%2Fsavernet%2Fpost%2F57d15f46-281d-4e74-afeb-a0306974e6be%2Fupdate%ED%9B%84.PNG)

이처럼 새로운 version의 row가 맨 마지막에 insert 되었습니다.<br>
만약 테이블 space의 중간에 공간이 있으면 해당 위치에 insert가 되기 때문에 무조건 맨 마지막에 생성이 되지 않습니다.<br>
위의 이유때문에 order by절을 쓰지 않으면 update 진행 후 row의 순서가 달라질 수 있습니다.

## MVCC

다중 버전 동시성 제어(Multi Version Concurrency Control)는 Lock을 사용하지 않고 읽기 일관성을 보장하는 방법입니다.

DBMS는 데이터를 update 하면 새로운 SCN(또는 rowversion)을 생성합니다. 만약 아직 commit을 하지 않은 상태에서 다른 세션에서 해당 데이터를 조회하게 된다면, update 이전의 SCN을 조회하기 때문에 읽기 일관성을 보장하고 기존 SCN의 row는 lock이 걸리지 않아 동시성을 유지할 수 있습니다.

MVCC는 두가지 방법이 있습니다.

1. 새로운 버전의 row를 생성하는 방법
update가 일어나게 되면 해당 row를 새로 insert 하는 방식
새로운 row를 insert 하기 때문에 물리적 위치가 변경된다.

   - PostgreSQL

2. 기존 버전의 SCN을 변경하는 방법
update가 일어나게 되면 해당 row의 SCN을 증가시키고 기존 데이터 블럭을 update한 내용으로 변경하고 기존 row를 특정 장소에 저장하는 방식<br>
Orcale은 undo 세그먼트에, MS-SQL은 tempdb에, MySQL은 undo log에 저장.<br>
SCN을 증가시키기 때문에 물리적 위치가 변경되지 않는다.

  - Oracle, MS-SQL, MySQL(InnoDB)

DBMS|update후 정렬순서 보장|Online DDL|Hint 사용|기본 Isolation Level|Index Null 저장
Oracle|보장|지원(12C 이상)|가능|Read Committed|X
MS-SQL|보장|지원(Server 2016 이상)|가능|Read Committed|O
MySQL|보장|지원(5.6 이상)|가능|Repeatable Read|O
PostgreSQL|보장 안함|미지원|불가능(PG_HINT_PLAN 사용)|Read Committed|X

#### 원문링크

[[DB] DBMS별 특징과 차이][link1]

[link1]: https://velog.io/@savernet/DB-DBMS%EB%B3%84-%EC%B0%A8%EC%9D%B4 "link1"