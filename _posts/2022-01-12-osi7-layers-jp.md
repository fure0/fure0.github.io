---
layout: post
categories: "network"
title: "OSI 7 Layers"
author: "TY_K"
---

<style>
    .post img {
        margin : 0
    }
</style>

![OSI_7_Layers](https://user-images.githubusercontent.com/20508342/78373084-11934680-7605-11ea-998f-8e82e05dbc05.png){: width="80%" height="80%"}

# OSI 7 계층(OSI 7 Layers)란?

## 7계층은 왜 구분할까？
통신이 일어나는 과정을 단계별로 알 수 있고, 특정한 곳에 이상이 생기면 그 단계만 수정할 수 있기 때문이다.


## 1) 물리(Physical)
> 리피터, 케이블, 허브 등

단지 데이터 전기적인 신호로 변환해서 주고받는 기능을 진행하는 공간

즉, 데이터를 전송하는 역할만 진행한다.

## 2) 데이터 링크((Data Link)
> 브릿지, 스위치 등

물리 계층으로 송수신되는 정보를 관리하여 안전하게 전달되도록 도와주는 역할

Mac 주소를 통해 통신한다. 프레임에 Mac 주소를 부여하고 에러검출, 재전송, 흐름제어를 진행한다.

## 3) 네트워크(Network)
> 라우터, IP

데이터를 목적지까지 가장 안전하고 빠르게 전달하는 기능을 담당한다.

라우터를 통해 이동할 경로를 선택하여 IP 주소를 지정하고, 해당 경로에 따라 패킷을 전달해준다.

라우팅, 흐름 제어, 오류 제어, 세그먼테이션 등을 수행한다.

## 4) 전송(Transport)
> TCP, UDP

TCP와 UDP 프로토콜을 통해 통신을 활성화한다. 포트를 열어두고, 프로그램들이 전송을 할 수 있도록 제공해준다.

TCP : 신뢰성, 연결지향적

UDP : 비신뢰성, 비연결성, 실시간

## 5) 세션(Session)
> API, Socket

데이터가 통신하기 위한 논리적 연결을 담당한다. TCP/IP 세션을 만들고 없애는 책임을 지니고 있다.

## 6) 표현(Presentation)
> JPEG, MPEG 등

데이터 표현에 대한 독립성을 제공하고 암호화하는 역할을 담당한다.

파일 인코딩, 명령어를 포장, 압축, 암호화한다.

## 7) 応用(Application)
> HTTP, FTP, DNS 등

최종 목적지로, 응용 프로세스와 직접 관계하여 일반적인 응용 서비스를 수행한다.

사용자 인터페이스, 전자우편, 데이터베이스 관리 등의 서비스를 제공한다.

## 상황 예
 PC방에서 게임을 하는데 연결이 끊겼다.
 어디에 문제가 있는지 확인하기 위해서는
 모든 PC가 문제가 있다면
 라우터의 문제(3계층 네트워크 계층)이거나 광랜을 제공하는 회사의 회선 문제(1계층 물리 계층)

 한 PC만 문제가 있고  
 오버워치 소프트웨어에 문제가 있다면(7계층 어플리케이션 계층)
 오버워치 소프트웨어에 문제가 없고, 스위치에 문제가 있으면(2계층 데이터링크 계층)
 있다고 판단해 다른 계층에 있는 장비나 소프트웨어를 건들이지 않는것이다.



[참고링크1][osi7-1]

[참고링크2][osi7-2]

[osi7-1]: https://github.com/gyoogle/tech-interview-for-developer/blob/master/Computer%20Science/Network/OSI%207%20%EA%B3%84%EC%B8%B5.md "osi7-1"

[osi7-2]: https://shlee0882.tistory.com/110 "osi7-2"