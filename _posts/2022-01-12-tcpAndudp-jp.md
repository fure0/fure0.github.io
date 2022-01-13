---
layout: post
categories: "network"
title: "TCP and UDP"
author: "TY_K"
---

### TCP (Transmission Control Protocol)

- 신뢰성이 높고 느린 프로토콜

- 연결의 설정(3-way handshaking)과 해제(4-way handshaking)
  
- 데이터 흐름 제어(수신자 버퍼 오버플로우 방지) 및 혼잡 제어(네트워크 내 패킷 수가 과도하게 증가하는 현상 방지)

- 1:1통신


### 특징

- 가상 회선 연결 방식, 연결형 서비스를 제공

- 높은 신뢰성(Sequence Number, Ack Number를 통한 신뢰성 보장)

- 연결의 설정(3-way handshaking)과 해제(4-way handshaking)

- 데이터 흐름 제어(수신자 버퍼 오버플로우 방지) 및 혼잡 제어(네트워크 내 패킷 수가 과도하게 증가하는 현상 방지)

- 전이중(Full-Duplex), 점대점(Point to Point) 서비스

### 소켓 통신 과정

- 서버 : 소켓을 생성, 주소 할당, 연결 요청 기다림, 요청에 대한 응답

- 클라이언트 : 소켓을 생성, 주소 할당, 연결 요청

### UDP (User Datagram Protocol)

- 신뢰성은 낮지만 빠른 프로토콜

- UDP는 비연결형 프로토콜로, 손상된 데이터에 대해서 재전송 하지 않음

- 대신에 신뢰성이 낮지만, TCP보다 속도가 빨라서 스트리밍 같은 서비스에 주로 사용됨

- 1:1, 1:N, N:M으로 연결이 가능


### 특징

- 비연결형(port만 확인하여 소켓을 식별하고 송수신)

- 패킷 오버헤드가 적어 네트워크 부하 감소

- 비신뢰성

- 오류검출(헤더에 오류 검출 필드를 포함하여 무결성 검사)

- TCP의 handshaking 같은 연결 설정이 없다

- DNS, NFS, SNMP, RIP 등 사용

### 소켓 통신 과정

- 서버 : 소켓을 생성, 주소 할당, 데이터를 송수신

- 클라이언트 : 소켓 생성 후 데이터 수신

UDP는 TCP와 달리 데이터의 수신에 대한 책임을 지지 않는다.

이는 송신자는 정보를 보냈지만, 정보가 수신자에게 제때에 도착했는지 또는 정보 내용이 서로 뒤바뀌었는지에 관해서 송신자는 상관할 필요가 없다.

TCP보다 안정성 면에서는 떨어지지만, 속도는 훨씬 빠르다.

[참고링크][tcp&udp]

[tcp&udp]: https://hahahoho5915.tistory.com/13 "tcp&udp"