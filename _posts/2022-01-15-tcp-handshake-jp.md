---
layout: post
categories: "network"
title: "TCP-HandShake"
author: "TY_K"
---

<style>
    .post img {
        margin : 0
    }
</style>

# [TCP] 3 way handshake & 4 way handshake

> 연결을 성립하고 해제하는 과정

## 3 way handshake - 연결성립

> TCP는 정확한 전송을 보장해야 한다. 따라서 통신하기에 앞서, 논리적인 접속을 성립하기 위해 3 way handshake 과정을 진행한다.

![3wayHandShake](https://user-images.githubusercontent.com/20508342/78797014-18013400-79f2-11ea-9921-b626f346e8cb.png)

1. 클라이언트가 서버에게 SYN 패킷을 보냄 (sequence : x)

2. 서버가 SYN(x)을 받고, 클라이언트로 받았다는 신호인 ACK와 SYN 패킷을 보냄 (sequence : y, ACK : x + 1)

3. 클라이언트는 서버의 응답은 ACK(x+1)와 SYN(y) 패킷을 받고, ACK(y+1)를 서버로 보냄

이렇게 3번의 통신이 완료되면 연결이 성립된다. (3번이라 3 way handshake인 것)

## 4 way handshake - 연결 해제

> 연결 성립 후, 모든 통신이 끝났다면 해제해야 한다.

![4wayHandShake](https://user-images.githubusercontent.com/20508342/78797054-26e7e680-79f2-11ea-82a7-355f7ec9e9be.png)

1. 클라이언트는 서버에게 연결을 종료한다는 FIN 플래그를 보낸다.

2. 서버는 FIN을 받고, 확인했다는 ACK를 클라이언트에게 보낸다. (이때 모든 데이터를 보내기 위해 CLOSE_WAIT 상태가 된다)

3. 데이터를 모두 보냈다면, 연결이 종료되었다는 FIN 플래그를 클라이언트에게 보낸다.

4. 클라이언트는 FIN을 받고, 확인했다는 ACK를 서버에게 보낸다. (아직 서버로부터 받지 못한 데이터가 있을 수 있으므로 TIME_WAIT을 통해 기다린다.)

* 서버는 ACK를 받은 이후 소켓을 닫는다 (Closed)

* TIME_WAIT 시간이 끝나면 클라이언트도 닫는다 (Closed)

이렇게 4번의 통신이 완료되면 연결이 해제된다.

## TCP Flag (URG, ACK, PSH, RST, SYN, FIN)

* SYN(Synchronization:동기화) - S : 연결 요청 플래그

    TCP 에서 세션을 성립할 때  가장먼저 보내는 패킷, 시퀀스 번호를 임의적으로 설정하여 세션을 연결하는 데에 사용되며 초기에 시퀀스 번호를 보내게 된다.

* ACK(Acknowledgement) - Ack : 응답

    상대방으로부터 패킷을 받았다는 걸 알려주는 패킷, 다른 플래그와 같이 출력되는 경우도 있습니다.
    받는 사람이 보낸 사람 시퀀스 번호에 TCP 계층에서 길이 또는 데이터 양을 더한 것과 같은 ACK를 보냅니다.(일반적으로 +1 하여 보냄) ACK 응답을 통해 보낸 패킷에 대한 성공, 실패를 판단하여 재전송 하거나 다음 패킷을 전송한다.

* RST(Reset) - R : 제 연결 종료

    재설정(Reset)을 하는 과정이며 양방향에서 동시에 일어나는 중단 작업이다. 비 정상적인 세션 연결 끊기에 해당한다. 이 패킷을 보내는 곳이 현재 접속하고 있는 곳과 즉시 연결을 끊고자 할 때 사용한다.

* PSH(Push) - P : 밀어넣기

    TELNET 과 같은 상호작용이 중요한 프로토콜의 경우 빠른 응답이 중요한데, 이 때 받은 데이터를 즉시 목적지인 OSI 7 Layer 의 Application 계층으로 전송하도록 하는 FLAG. 대화형 트랙픽에 사용되는 것으로 버퍼가 채워지기를 기다리지 않고 데이터를 전달한다. 데이터는 버퍼링 없이 바로 위 계층이 아닌 7 계층의 응용프로그램으로 바로 전달한다.

* URG(Urgent) - U : 긴급 
 
    Urgent pointer 유효한 것인지를 나타낸다. Urgent pointer란 전송하는 데이터 중에서 긴급히 전당해야 할 내용이 있을 경우에 사용한다. 긴급한 데이터는 다른 데이터에 비해 우선순위가 높아야 한다. 
    <br/>EX) ping 명령어 실행 도중 Ctrl+c 입력

* FIN(Finish) - F : 연결 종료 요청

    세션 연결을 종료시킬 때 사용되며 더이상 전송할 데이터가 없음을 나타낸다.

#### 원문링크    

[[TCP] 3 way handshake & 4 way handshake][TCP-HandShake]

[TCP-HandShake]: https://github.com/gyoogle/tech-interview-for-developer/blob/master/Computer%20Science/Network/TCP%203%20way%20handshake%20%26%204%20way%20handshake.md "TCP-HandShake"

[TCP flag(URG, ACK, PSH, RST, SYN, FIN)][TCP-Flag]

[TCP-Flag]: https://mindgear.tistory.com/206 "TCP-Flag"
