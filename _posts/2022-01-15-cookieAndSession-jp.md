---
layout: post
categories: "web"
title: "Cookie and Session"
author: "TY_K"
---

<style>
    table {
        border-collapse: collapse;
    }
    table tr td{
        border:1px solid;
    }
    table th:first-of-type {
        width: 15%;
    }
    table th:nth-of-type(2) {
        width: 45%;
    }
    table th:nth-of-type(3) {
        width: 40%;
    }
</style>

### 쿠키와 세션을 사용하는 이유

HTTP 프로토콜의 특징이자 약점을 보완하기 위해서 사용

### 1 - Connectionless 프로토콜 (비연결지향)

클라이언트가 서버에 요청(Request)을 했을 때,
그 요청에 맞는 응답(Response)을 보낸 후 연결을 끊는 처리방식이다.

HTTP 1.1 버전에서 연결을 유지하고, 재활용 하는 기능이 Default 로 추가되었다.
(keep-alive 값으로 변경 가능)

### 2 - Stateless 프로토콜 (상태정보 유지 안함)

클라이언트의 상태 정보를 가지지 않는 서버 처리 방식이다.

클라이언트와 첫번째 통신에서 데이터를 주고 받았다 해도,
두번째 통신에서 이전 데이터를 유지하지 않는다.

### 실제로는 데이터 유지가 필요한 경우가 많다.

정보가 유지되지 않으면, 매번 페이지를 이동할 때마다 로그인을 다시 하거나,
상품을 선택했는데 구매 페이지에서 선택한 상품의 정보가 없거나 하는 등의 일이 발생할 수 있다.

따라서, Stateful 경우를 대처하기 위해서 쿠키와 세션을 사용한다.

쿠키와 세션의 차이점은 크게 상태 정보의 저장 위치이다.

쿠키는 '클라이언트(=로컬PC)'에 저장하고, 세션은 '서버' 에 저장한다.

서버와 클라이언트가 통신을 할 때 통신이 연속적으로 이어지지 않고 한 번 통신이 되면 끊어진다.

따라서 서버는 클라이언트가 누구인지 계속 인증을 해줘야 한다. 하지만 그것은 매우 귀찮고 번거로운 일이다. 

또한 웹페이지의 로딩을 느리게 만드는 요인이 되기도 한다. 그런 번거로움을 해결하는 방법이 바로 쿠키와 세션이다.

정리하면, 클라이언트와 정보 유지를 하기 위해 사용하는 것이 쿠키와 세션이다.

### 세션을 쓰면되는데 굳이 쿠키를 사용하는 이유?

세션이 쿠키에 비해 보안도 높은 편이나 쿠키를 사용하는 이유는
세션은 서버에 저장되고, 서버자원을 사용하기 때문에 사용자가 많을 경우 소모되는 자원이 상당하다.

이러한 자원관리 차원에서 쿠키와 세션을 적절한 요소 및 기능에 병행 사용하여,
서버 자원의 낭비를 방지하며 웹사이트의 속도를 높일 수 있다.

### 쿠키(Cookie)

HTTP의 일종으로 사용자가 어떠한 웹 사이트를 방문할 경우,
그 사이트가 사용하고 있는 서버에서 사용자의 컴퓨터에 저장하는 작은 기록 정보 파일이다.

HTTP에서 클라이언트의 상태 정보를 클라이언트의 PC에 저장하였다가
필요시 정보를 참조하거나 재사용할 수 있다.

### 쿠키 특징

* 이름, 값, 만료일(저장 기간 설정), 경로 정보로 구성되어 있다.
* 클라이언트에 총 300개의 쿠키를 저장할 수 있다
* 하나의 도메인 당 20개의 쿠키를 가질 수 있다
* 하나의 쿠키는 4KB(=4096byte)까지 저장 가능하다.
* HTTP 헤더에 포함되어 전송

### 쿠키의 동작 순서

a. 클라이언트가 페이지를 요청한다. (사용자가 웹사이트 접근)

b. 웹 서버는 쿠키를 생성한다.

c. 생성한 쿠키에 정보를 담아 HTTP 화면을 돌려줄 때, 같이 클라이언트에게 돌려준다.

d. 넘겨 받은 쿠키는 클라이언트가 가지고 있다가(로컬 PC에 저장) 다시 서버에 요청할 때 요청과 함께 쿠키를 전송한다.

e. 동일 사이트 재방문시 클라이언트의 PC에 해당 쿠키가 있는 경우, 요청 페이지와 함께 쿠키를 전송한다.

### 세션(Session)

일정 시간동안 같은 사용자(브라우저)로부터 들어오는
일련의 요구를 하나의 상태로 보고, 그 상태를 일정하게 유지시키는 기술이다.

여기서 일정 시간은 방문자가 웹 브라우저를 통해
웹 서버에 접속한 시점으로부터 웹 브라우저를 종료하여 연결을 끝내는 시점을 말한다.

즉, 방문자가 웹 서버에 접속해 있는 상태를 하나의 단위로 보고 그것을 세션이라고 한다.

### 세션 특징

* 웹 서버에 웹 컨테이너의 상태를 유지하기 위한 정보를 저장한다.
* 웹 서버의 저장되는 쿠키(=세션 쿠키)
* 브라우저를 닫거나, 서버에서 세션을 삭제했을때만 삭제가 되므로, 쿠키보다 비교적 보안이 좋다.
* 저장 데이터에 제한이 없다.(서버 용량이 허용하는 한...)
* 각 클라이언트 고유 Session ID를 부여한다. Session ID로 클라이언트를 구분하여 각 클라이언트 요구에 맞는 서비스 제공

### 세션의 동작 순서

a. 클라이언트가 페이지를 요청한다. (사용자가 웹사이트 접근)

b. 서버는 접근한 클라이언트의 Request-Header 필드인 Cookie를 확인하여, 클라이언트가 해당 session-id를 보냈는지 확인한다.

c. session-id가 존재하지 않는다면, 서버는 session-id를 생성해 클라이언트에게 돌려준다.

d. 서버에서 클라이언트로 돌려준 session-id를 쿠키를 사용해 서버에 저장한다.

쿠키 이름 : JSESSIONID

e. 클라이언트는 재접속 시, 이 쿠키(JSESSIONID)를 이용하여 session-id 값을 서버에 전달

||Cookie|Session|
|---|---|---|
|저장 위치|클라이언트(=접속자 PC)|웹 서버|
|저장 형식|text|Object|
|만료 시점|쿠키 저장시 설정(브라우저가 종료되도, 만료시점이 지나지 않으면 자동삭제되지 않음)|브라우저 종료시 삭제(기간 지정 가능)|
|사용하는 자원|클라이언트 리소스|웹 서버 리소스|
|용량 제한|총 300개/하나의 도메인 당 20개/하나의 쿠키 당 4KB(=4096byte)|서버가 허용하는 한 용량제한 없음|
|속도|세션보다 빠름|쿠키보다 느림|
|보안|세션보다 안좋음|쿠키보다 좋음|

<br>

[참고링크][cookie]

[cookie]: https://hahahoho5915.tistory.com/32 "cookie"