---
published: true 
layout: post
categories: "DB"
title: "DB Replication and Clustering"
author: "TY_K"
---

## 클러스터링(Clustering)이란?

![image1](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbubYWG%2FbtrzLxxpAWU%2FLWiOLKKdoHfWuBJ0LGhAxK%2Fimg.webp){: width="50%" height="50%"}

클러스터링은 동일한 데이터베이스를 여러 대의 서버가 관리하도록 클러스터를 구축하는 것을 뜻한다. 
<br>이러한 클러스터링은 Active-Active 방식과 Active-StandBy 방식이 있다.

### 클러스터링을 하는 이유

그림과 같이 모든 DB 서버가 Active 상태면 하나의 서버에 이상이 생기더라도 바로 다른 서버를 이용해 정상적인 서비스 운영이 가능하다.

또한 클러스터링을 이용하게 되면 기존에 하나의 서버에 몰리던 부하를 여러 곳으로 분산시킬 수 있다. 즉, 로드밸런싱(Load Balancing)이 가능해진다. 

### 클러스터링의 단점

하지만 클러스터링은 여러대의 서버가 데이터베이스를 공유하므로 병목현상이 발생해 더 많은 비용이 발생할 수 있다.

특히, Active-Active 방식은 모든 서버가 활성화된 상태이므로 병목 현상이 더 심하게 발생할 수 있다.<br> 이러한 단점을 완화시킬 수 있는 방식이 바로 Active-StandBy 방식이다.

![image2](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fcdl7Vp%2FbtrzQWPw8RK%2Fk0FY4ief1lASqctTFcu0pk%2Fimg.webp){: width="50%" height="50%"}

Active-StandBy 방식은 Active 상태인 서버와 Stand-By 상태인 서버를 나누어 운영한다. 

즉, Stand-By 상태의 서버는 말 그대로 준비 상태로 대기하다 Active 서버에 문제가 발생했을 경우 Active 상태로 전환되어 사용된다. <br>이러한 Active-Stand-By 방식은 병목현상을 해결할 수 있다.

하지만, 장애가 발생했을 경우 Stand-By 상태에서 Active 상태로 전환하는 시간동안 서비스를 사용할 수 없다는 단점도 존재한다. 

또한, Active-Active는 여러 대의 서버를 온전히 사용하여 부하를 줄일 수 있는 장점이 있지만, Active-StandBy 방식은 부하 분산 기능의 효율이 줄어드는 단점이 있다.

이러한 클러스터링 방식은 DB 서버의 장애를 Fail-Over하는 방식이라고 할 수 있다. 

하지만 만약 데이터베이스에서 장애가 발생한다면 어떻게 극복할 수 있을까?<br> 이때 사용할 수 있는 방식 중 하나가 리플리케이션(Replication)이다.

## 리플리케이션(Replication)이란?

![image3](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FRcknO%2FbtrzN2Xhlcg%2FP1DA9AMTO0akvKpUl88WI0%2Fimg.webp){: width="50%" height="50%"}

데이터베이스 리플리케이션(Replication)이란 말 그대로 복제본 데이터베이스를 운용하는 것이다. 

원본 데이터베이스를 Master, 복제된 데이터베이스를 Slave라고 부른다. <br>Slave 데이터베이스는 Master 데이터베이스를 복제(Replication)하여 동일한 데이터를 가지게 된다.

### 리플리케이션을 하는 이유

이러한 리플리케이션을 사용하면 얻을 수 있는 이점이 있다. 

그림에서 볼 수 있듯이 Master 데이터베이스와 Slave 데이터베이스에 각기 다른 명령을 수행하도록 할 수 있다. 

Insert, Update, Delete 등의 작업은 Master 데이터베이스에서 처리하고, Select 등의 조회 작업은 Slave 데이터베이스에서 처리하도록 하면, 기존에 너무 많던 부하를 분산시킬 수 있다. 

또한 리플리케이션을 이용하면 데이터 안정성을 획득 할 수 있다. Master 데이터베이스의 데이터가 손상되었을지라도 Slave 데이터베이스에 복제된 데이터를 통하여 복구할 수 있다.

### 리플리케이션의 단점

하지만 리플리케이션이 마냥 좋기만 한것은 아니다. 리플리케이션이 이루어진 Slave 데이터베이스는 비동기(Asynchronous) 방식으로 Master 데이터베이스와 동기화 한다. 

따라서 두 데이터베이스 간의 데이터 동기화가 보장되지 않아 일관성에 문제가 생길 수 있다. 또한, Master 데이터베이스가 정상 작동하지 않는다면 복구 및 대처가 까다롭다.

#### 원문링크

[[DB] 리플리케이션(Replication)이란? 클러스터링(Clustering)이란?][link1]

[link1]: https://code-lab1.tistory.com/205 "link1"