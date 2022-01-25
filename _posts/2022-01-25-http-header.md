---
layout: post
categories: "web"
title: "Http Header"
author: "TY_K"
---

HTTP 헤더는 클라이언트와 서버가 요청 또는 응답으로 부가적인 정보를 전송할 수 있도록 한다.

![image1](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FY9FVZ%2FbtqKxk03h39%2FXXb8ovXh9wpnHXNXS5ExRk%2Fimg.png)

HTTP 메시지는 보통 Header + Body로 이루어지는데,

![image2](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FnlJLJ%2FbtqKuGjWbh8%2FjistXkxmEvfQLnNdeq7g60%2Fimg.png)

그리고 그 헤더에는

![image3](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb4UKNp%2FbtqKv6Cuscr%2F1AkUVBUnrTYNiybGBS0yJ1%2Fimg.png)

위와 같이 Requset 와 Response로 나뉘어진다.

이제 어떤 것들이 있는지 확인해보았으니 그 항목에 대해서 또 알아보자.

### <span style="color:Green">Header</span>

헤더는 크게 4가지로 분류된다.

* General Header(공통 헤더)
* Request Header(요청 헤더)
* Response Header(응답 헤더)
* Entity Header(엔티티 헤더)

### <span style="color:Green">General Header</span>

공통 헤더는 요청 및 응답의 메시지 모두에서 사용되지만 컨텐츠에는 적용되지 않는 헤더이다.

흔하게 우리가 사용하는 공통 헤더는 Date, Cache-Control, Connection등이 있다.

#### Date

일반적인 HTTP 헤더는 만들어진 날짜와 시간을 포함하는데, 다음과 같다.

```html
General-Header: Data: <day-name>, <day> <month> <year> <hour>:<minute>:<second> 
GMT

// Date: Wed, 21 Oct 2015 07:28:00 GMT 
```

요청과 응답시에 자동으로 만들어지는 헤더이다.

#### Connection

Connection 헤더는 현재의 전송이 완료된 후 네트워크 접속을 유지할지 말지를제어한다.

Connection의 속성에는 크게 2가지가 있다.

1. Keep-alive : 지속 연결
2. close : 연결 종료

```html
Connection: keep-alive
Connection: close
```

#### Cache-Control

Cache-Control 헤더는 캐싱을 허용할지 허용하지 않을지를 정하기 위해 사용된다.

### <span style="color:Green">Request Header</span>

요청 헤더는 HTTP 요청에서 사용되지만 메시지의 컨텐츠와 관련이 없는 HTTP 헤더이다.

보통 Fetch될 리소스나 클라이언트 자체에 대한 정보를 포함하여 서버로 보내진다.

#### Host

![host1](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FGphDU%2FbtqKvQmm0JI%2F8Y3WeGkmipkvD1s7ibhoK1%2Fimg.png)

서버의 도메인 네임과 서버가 현재 Listening 중인 TCP 포트를 지정한다.

만약 포트가 지정되지 않는다면 요청된 서버의 기본 포트를 의미한다. (HTTP URL은 80)

Host 헤더는 반드시 하나가 존재해야 한다.

만약 한 개가 아니라 없거나 여러개면 400(Bad Request) 상태 코드가 전송된다.

```html
Host: <host>:<port_Optional>

// Host: developer.cdn.mozilla.net
```
#### User-Agent

현재 사용자가 어떤 클라이언트(운영체제와 브라우저를 포함한)를 이용해 요청을 보냈는지 확인할 수 있다.

```html
User-Agent: <product> / <product-version> <comment>

// Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/51.0.2704.103 Safari/537.36
// Mozilla/5.0 (iPhone; CPU iPhone OS 13_5_1 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/13.1.1 Mobile/15E148 Safari/604.1
// Mozilla/5.0 (compatible; Googlebot/2.1; +http://www.google.com/bot.html)
```

![user-agent1](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FDTDgI%2FbtqKwRdTUa6%2FJpNHXirhg45Zz0tU35Cbf0%2Fimg.png)

#### Accept

Accept는 요청을 보낼 때 서버에게 어떤 터입으로 응답을 보내줬으면 좋겠다고 명시한다.

예를 들어
```html
Accept: text/html
```

을 요청 헤더로 보내면 HTML 형식인 응답을 처리하겠다는 뜻이다.

이는 콤마로 여러 타입을 동시에 적어줄 수도 있으며 와일드카드인 * 아스테리크로 보낼 수도 있다.

이러한 Accept에는 다음과 같은 것들도 포함될 수 있다.

1. Accept-Charset : 원하는 Character Set
2. Accept-Language : 원하는 Lang
3. Accept-Encoding : 원하는 Encoding 방식

![accept1](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FQJ6eS%2FbtqKwREYzE6%2FNX7xrdhP7LLKKC3clPNSz0%2Fimg.png)

대충 브라우저는 알아서 설정해서 보내는 Accept가 있다.

무슨 헤더를 적어야할지 모르겠다면 브라우저가 사용하는 것을 이용하자.

#### Authorization

Authorization 헤더는 사용자가 서버에 인가된 사용자임을 증명할 때 사용한다.

보통 JWT나 Bearer 토큰을 서버로 보낼 때 사용하며 자격이 증명되지 않을 경우 401 Unauthorized 상태를 알려준다.

```html
Authorization: <type> <credentials>

// Authorization: Basic YWxhZGRpbjpvcGVuc2VzYW1l
```

* type : 인증 타입으로 보통 Basic과 Bearer을 사용한다.
* credentials : 사용자명과 비밀번호, 다른 페이로드가 합처져 base64로 인코딩된 값

> Base64 인코딩은 암호화나 해싱을 의미하지 않습니다! 이 방법은 인증에 대해서 문자를 그대로 보내는 것과 동일하다고 할 수 있습니다 (base64인코딩은 복호화 가능). Basic 인증을 하는 경우 HTTPS로 접속하는 것을 권장합니다. by MDN

#### Origin

POST와 같은 요청을 보낼 때, 요청이 어느 주소에서 시작되었는지를 나타낸다.

여기서 요청을 보낸 주소와 받는 주소가 다르면 CORS 문제가 발생하는데, CORS에 대해 모르겠다면 CORS란?이라는 글로 가서 확인하길 바란다.

```html
Origin: null
Origin: <scheme> "://" <hostname> [ ":" <port> ]

// Origin: https://developer.mozilla.org
```
* scheme : 사용하는 프로토콜
* hostname : 서버의 이름 또는 ip
* port : 서버에 열린 tcp 포트

#### Referer

이는 이 페이지 이전에 대한 주소가 담겨있다.

이 헤더를 이용하면 어떤 페이지에서 지금 페이지로 왔느지를 알 수 있어서 애널리틱스를 하는데 많이 사용된다.

```html
Referer: <url>

Referer: https://developer.mozilla.org/en-US/docs/Web/JavaScript
```

![referer1](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FockIX%2FbtqKxmdEUZl%2FalBEnWjsPbKy3YG0h0dAm1%2Fimg.png)

### <span style="color:Green">Response Header</span>

위치 또는 서버 자체에 대한 정보(이름, 버전)과 같이 응답에 대한 부가적인 정보를 갖는 헤더이다.

#### Access-Control-Allow-Origin

요청을 보내는 클라이언트의 주소와 요청을 받는 백엔드 주소가 다르면 CORS 에러가 발생한다

이 때 Access-Control-Allow-Origin 헤더에 클라이언트의 주소를 적어야 에러가 발생하지 않는다.

만약

1. 프로토콜
2. 서브도메인
3. 도메인
4. 포트

중 하나라도 다르면 CORS 에러가 발생한다.

```html
Access-Control-Allow-Origin: *
Access-Control-Allow-Origin: <origin>
Access-Control-Allow-Origin: null

// Access-Control-Allow-Origin: https://developer.mozilla.org
```

주소를 일일지 지정하기 싫다면 *로 모든 주소에 CORS 요청을 허용할 수 있지만 보안이 취약해지는 점이 있기 때문에 해당 헤더를 사용하는 것으로 하자.

이와 유사한 헤더로는

```html
Access-Control-Request-Method
Access-Control-Request-Headers
Access-Control-Allow-Methods
Access-Control-Allow-Headers
```

등이 있다.

#### Allow

Allow 헤더는 Access-Control-Allow-Mehtods랑 비슷하지만 CORS 요청 외에도 적용된다는 특징이 있다.

특정 메서드만 허용하겠다라는 것을 지정하는 헤더이다.

```html
Allow: <http-methods>

// Allow: GET, POST, HEAD
```

#### Content-Disposition

응답 본문을 브라우저가 어떻게 표시해야 할지 알려주는 헤더이다.

inline인 경우 웹페이지 화면에 표시되고, attachment인 경우 다운로드된다.

```html
Content-Disposition: inline
Content-Disposition: attachment
Content-Disposition: attachment; filename="filename.jpg"

// ex
POST /test.html HTTP/1.1
Host: example.org
Content-Type: multipart/form-data;boundary="boundary"

--boundary
Content-Disposition: form-data; name="field1"

value1
--boundary
Content-Disposition: form-data; name="field2"; filename="example.txt"

value2
--boundary--
```

다운로드되길 원하는 파일은 attachment로 값을 설정하고, filename 옵션으로 파일명까지 지정해줄 수 있다.

보통 FTP 서버나 파일 전용 서버에서 많이 사용하게 된다.

#### Location

리다이렉트 헤더라고도 하며, 300번대 응답이나 201 Created 응답일 때 어느 페이지로 이동할지를 알려주는 헤더이다.

```html
Location: <url>

// ex
HTTP/1.1 302 Found
Location: /
```

#### Content-Security-Policy

외부 파일들을 불러올 경우, 차단할 소스와 불러올 소스를 명시할 수 있다.

이 헤더를 이용한다면 XSS 공격과 같은 잠재적 공격 소스 파일을 차단할 수 있고, 잘 동작하는 소스 코드를 허용할 수 있게 된다.

만얀 이 값을 self라고 지정한다면 자신과 같은 도메인의 파일만 가져 올 수 있게 되는 것이다

```html
Content-Security-Policy: <policy-directive>; <policy-directive>

// Content-Security-Policy: default-src https:
```

### <span style="color:Green">Entity Header</span>

컨텐츠 길이나 MIME 타입과 같이 Entity Body에 대한 자세한 정보를 포함하는 헤더이다.

헤더는 프록시의 처리 방법에 따라 그룹핑될 수 있습니다.

#### Content-Length

Content-Length는 바이트 단위를 가지는 Header + Body의 크기를 나타낸다.
메시지 크기에 따라 자동으로 생성된다.

```html
Content-Length: <length>
```

![content-length1](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FGphDU%2FbtqKvQmm0JI%2F8Y3WeGkmipkvD1s7ibhoK1%2Fimg.png)

#### Content-Type

Content-Type는 개체의 미디어 타입(MIME)과 문자열 인코딩(UTF-8)을 지정하기 위해 사용한다.
이는 Accept 헤더와 대웅된다.

```html
// 메시지 내용은 text/html 타입이고 문자열은 utf-8 문자열이라는 것
Content-Type: text/html; charset=utf-8
Content-Type: multipart/form-data; boundary=something
```

![content-type1](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FGphDU%2FbtqKvQmm0JI%2F8Y3WeGkmipkvD1s7ibhoK1%2Fimg.png)

#### Content-Language

Content-Language 헤더는 사용자들에게 언어를 설명하기 위해 사용된다.
사용자가 선호하는 언어에 따라 사용자를 구분할 수 있게 한다.
예를 들어서 Content-Language: en-US로 설정된다면, 미국에서 만들어졌지만, 일부분이 영어가 아닐 수도 있다.

만약 Content-Language가 지정되지 않으면 모든 사람들에게 공개된 정보라고 간주한다.
Content-Language는 Text 뿐만 아니라 미디어 타입에도 적용된다.

```html
Content-Language: de-DE
Content-Language: en-US
Content-Language: de-DE, en-CA
```

HTML 문서에도 다음과 같이 사용할 수 있지만 사용하지 않는것을 MDN에서 권고한다.

```html
<!-- /!\ This is bad practice -->
<meta http-equiv="content-language" content="de">
```

#### Content-Encoding

Content-Encoding 은 미디어 타입을 압축하기 위해서 사용된다.
이 헤더가 존재한다면 그 값은어떤 방식으로 인코딩 되는지 알 수 있다.
우리가 gzip등의 알고리즘을 통해 encoding 해서 보낸다면 브라우저가 알아서 해제해서 사용한다.

```html
Content-Encoding: gzip
Content-Encoding: compress
Content-Encoding: deflate
Content-Encoding: identity
Content-Encoding: br

// Content-Encoding: gzip
```

[참고링크1][link1]

[link1]: https://wonit.tistory.com/308 "link1"

[참고링크2][link2]

[link2]: https://wonit.tistory.com/309?category=749910 "link2"

[참고링크3][link3]

[link3]: https://wonit.tistory.com/310?category=749910 "link3"

[참고링크4][link4]

[link4]: https://wonit.tistory.com/311?category=749910 "link4"
