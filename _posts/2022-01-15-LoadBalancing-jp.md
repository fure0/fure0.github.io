---
layout: post
categories: "network"
title: "LoadBalancing"
author: "TY_K"
---

<style>
    .post img {
        margin : 0
    }
</style>

![LoadBalancing](https://user-images.githubusercontent.com/20508342/78453183-073b8000-76cb-11ea-902b-9fa82e8b06b7.png)


# 로드 밸런싱(Load Balancing)

> 둘 이상의 CPU or 저장장치와 같은 컴퓨터 자원들에게 작업을 나누는 것

요즘 시대에는 웹사이트에 접속하는 인원이 급격히 늘어나게 되었다.

따라서 이 사람들에 대해 모든 트래픽을 감당하기엔 1대의 서버로는 부족하다. 대응 방안으로 하드웨어의 성능을 올리거나(Scale-up) 여러대의 서버가 나눠서 일하도록 만드는 것(Scale-out)이 있다. 하드웨어 향상 비용이 더욱 비싸기도 하고, 서버가 여러대면 무중단 서비스를 제공하는 환경 구성이 용이하므로 Scale-out이 효과적이다. 이때 여러 서버에게 균등하게 트래픽을 분산시켜주는 것이 바로 로드 밸런싱이다.

## 로드 밸런서가 서버를 선택하는 방식

* 라운드 로빈(Round Robin) : CPU 스케줄링의 라운드 로빈 방식 활용

* Least Connections : 연결 개수가 가장 적은 서버 선택 (트래픽으로 인해 세션이 길어지는 경우 권장)

* Source : 사용자 IP를 해싱하여 분배 (특정 사용자가 항상 같은 서버로 연결되는 것 보장)

## 로드 밸런서 장애 대비

서버를 분배하는 로드 밸런서에 문제가 생길 수 있기 때문에 로드 밸런서를 이중화하여 대비한다.

## 몇개층에서 정보를 열어서 분산하는가?

크게 L4로드밸런서와 L7로드밸런서로 나눔

L4 로드밸런서는 4계층 이하의 정보를 가지고 로드를 분산

특히 MAC주소, IP주소, 포트정보를 가지고 트래픽을 분산

L7 로드밸런서는 응용 계층의 정보를 가지고 로드 분산

패킷 내용을 확인하고 분산해서 DDos같은 비정상적인 트래픽도 필터링할 수 있다.

> L7 로드밸런싱은 모든 정보를 다 알고 있다.
* L7: URL기반

> L4 로드밸런싱은 L4까지만 보고 요청을 분산 시켜준다
* L4: port기반
* L3: IP기반
* L2: MAC Address기반



[참고링크][LoadBalancing]

[LoadBalancing]: https://github.com/gyoogle/tech-interview-for-developer/blob/master/Computer%20Science/Network/%EB%A1%9C%EB%93%9C%20%EB%B0%B8%EB%9F%B0%EC%8B%B1(Load%20Balancing).md "LoadBalancing"