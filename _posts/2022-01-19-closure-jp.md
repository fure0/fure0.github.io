---
layout: post
categories: "JS"
title: "Closure"
author: "TY_K"
---

### Closure(클로저)란?
MDN에서는 클로저를 다음과 같이 정의하고 있다.

> 클로저는 독립적인 (자유) 변수를 가리키는 함수이다. 또는, 클로저 안에 정의된 함수는 만들어진 환경을 ‘기억한다’.

흔히 함수 내에서 함수를 정의하고 사용하면 클로저라고 한다. 하지만 대개는 정의한 함수를 리턴하고 사용은 바깥에서 하게된다. 말로 설명하면 설명하기가 복잡하니 우선 코드를 보자.

```javascript
function getClosure() {
  var text = 'variable 1';
  return function() {
    return text;
  };
}

var closure = getClosure();
console.log(closure()); // 'variable 1'
```

위에서 정의한 getClosure()는 함수를 반환하고, 반환된 함수는 getClosure() 내부에서 선언된 변수를 참조하고 있다. 또한 이렇게 참조된 변수는 함수 실행이 끝났다고 해서 사라지지 않았고, 여전히 제대로 된 값을 반환하고 있는 걸 알 수 있다.

여기서 반환된 함수가 클로저인데, MDN에서 정의된 내용에서도 말했듯 환경을 기억하고 있는 것처럼 보인다. 아직은 잘 이해가 되지 않으니, 다른 예제도 한 번 보겠다.

```javascript
var base = 'Hello, ';
function sayHelloTo(name) {
  var text = base + name;
  return function() {
    console.log(text);
  };
}

var hello1 = sayHelloTo('Tom');
var hello2 = sayHelloTo('James');
var hello3 = sayHelloTo('Tony');
hello1(); // 'Hello, Tom'
hello2(); // 'Hello, James'
hello3(); // 'Hello, Tony'
```

출력된 결과를 보면 text 변수가 동적으로 변화하고 있는 것처럼 보인다. 실제로는 text라는 변수 자체가 여러 번 생성된 것이다. 즉, hello1()과 hello2(), hello3()은 서로 다른 환경을 가지고 있다.

### 클로저를 통한 은닉화

일반적으로 JavaScript에서 객체지향 프로그래밍을 말한다면 Prototype을 통해 객체를 다루는 것을 말한다. ES2015에는 클래스도 있지만 이걸 이야기하면 너무 삼천포로 빠지므로 넘어가도록 하고..

여하튼 Prototype을 통한 객체를 만들 때의 주요한 문제 중 하나는 Private variables에 대한 접근 권한 문제이다. 예제 코드를 보자.

```javascript
function Hello(name) {
  this._name = name;
}

Hello.prototype.say = function() {
  console.log('Hello, ' + this._name);
}

var hello1 = new Hello('Tom');
var hello2 = new Hello('James');
var hello3 = new Hello('Tony');

hello1.say(); // 'Hello, Tom'
hello2.say(); // 'Hello, James'
hello3.say(); // 'Hello, Tony'
hello1._name = 'anonymous';
hello1.say(); // 'Hello, anonymous'
```

위에서 Hello()로 생성된 객체들은 모두 _name이라는 변수를 가지게 된다. 변수명 앞에 underscore(_)를 포함했기 때문에 일반적인 JavaScript 네이밍 컨벤션을 생각해 봤을때 이 변수는 Private variable으로 쓰고싶다는 의도를 알 수 있다. 하지만 실제로는 여전히 외부에서도 쉽게 접근가능한 변수일 뿐이다.

이 경우에 클로저를 사용하여 외부에서 변수에 직접 접근하는 것을 제한할 수 있다.

```javascript
function hello(name) {
  var _name = name;
  return function() {
    console.log('Hello, ' + _name);
  };
}

var hello1 = hello('Tom');
var hello2 = hello('James');
var hello3 = hello('Tony');

hello1(); // 'Hello, Tom'
hello2(); // 'Hello, James'
hello3(); // 'Hello, Tony'
```

특별히 인터페이스를 제공하는 것이 아니라면, 여기서는 외부에서 _name에 접근할 방법이 전혀 없다. 이렇게 은닉화도 생각보다 쉽게 해결할 수 있다.

### 반복문 클로저

닳고 닳은 예제이긴 하지만, 그렇다고 빠트리긴 아쉬운 예제이므로 여기서도 한 번 짚고 넘어가야 할 것 같다.
```javascript
var i;
for (i = 0; i < 10; i++) {
  setTimeout(function() {
    console.log(i);
  }, 100);
}
```

간단하게 0-9까지의 정수를 출력하는 코드이지만 실제로 돌려보면 엉뚱하게도 10만 열 번 출력되는 걸 볼 수 있다. 왜일까?

먼저 setTimeout()에 인자로 넘긴 익명함수는 모두 0.1초 뒤에 호출될 것이다. 그 0.1초 동안에 이미 반복문이 모두 순회되면서 i값은 이미 10이 된 상태. 그 때 익명함수가 호출되면서 이미 10이 되어버린 i를 참조하는 것이다.

이 경우에도 클로저를 사용하면 원하는 대로 동작하도록 만들 수 있다.

```javascript
var i;
for (i = 0; i < 10; i++) {
  (function(j) {
    setTimeout(function() {
      console.log(j);
    }, 100);
  })(i);
}
```
중간에 IIFE를 덧붙여 setTimeout()에 걸린 익명함수를 클로저로 만들었다. 앞서 말한대로 클로저는 만들어진 환경을 기억한다. 이 코드에서 i는 IIFE내에 j라는 형태로 주입되고, 클로저에 의해 각기 다른 환경속에 포함된다. 반복문은 10회 반복되므로 10개의 환경이 생길 것이고, 10개의 서로 다른 환경에 10개의 서로 다른 j가 생긴다.

이쯤에서 IIFE 매개변수로 i를 넘기지 않고 그냥 직접 참조해도 되지 않느냐는 의문이 들 수도 있다. 하지만 직접 그렇게 해보면 원하는 대로 동작하지 않는다.

위의 예제에서는 IIFE를 통해서 클로저마다 환경이 생긴다. 하지만 인자로 i를 넘기지 않는다면 당연히 클로저가 참조하는 IIFE의 함수 스코프에서도 i값이 없으므로 생성 당시의 외부 스코프인 글로벌을 탐색하게 되고 결국 모두 같은 i를 참조하게 된다. 반면에, 인자로 i를 넘기게 되면 IIFE로 만든 10개의 스코프에 모두 i라는 변수가 다른 값으로 생기므로 정상적으로 동작할 수 있는 것이다.

### 클로저의 성능

클로저는 각자의 환경을 가진다. 이 환경을 기억하기 위해서는 당연히 메모리가 소모될 것이다. 클로저를 생성해놓고 참조를 제거하지 않는 것은 C++에서 동적할당으로 객체를 생성해놓고 delete를 사용하지 않는 것과 비슷하다. 클로저를 통해 내부 변수를 참조하는 동안에는 내부 변수가 차지하는 메모리를 GC가 회수하지 않는다. 따라서 클로저 사용이 끝나면 참조를 제거하는 것이 좋다.

```javascript
function hello(name) {
  var _name = name;
  return function() {
    console.log('Hello, ' + _name);
  };
}

var hello1 = hello('Tom');
var hello2 = hello('James');
var hello3 = hello('Tony');

hello1(); // 'Hello, Tom'
hello2(); // 'Hello, James'
hello3(); // 'Hello, Tony'

// 여기서 메모리를 release 시키기 클로저의 참조를 제거해야 한다.
hello1 = null;
hello2 = null;
hello3 = null;
```
이처럼 메모리 관리에 있어서 약점이 있지만 추가로 스코프 체인을 검색하는 시간과 새로운 스코프를 생성하는데 드는 비용도 감안하지 않을 수 없다.

[참고링크][closure]

[closure]: https://hyunseob.github.io/2016/08/30/javascript-closure/ "closure"
