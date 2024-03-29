---
layout: post
categories: "JS"
title: "Hoisting"
author: "TY_K"
---

ES6 문법이 표준화가 되면서 크게 신경쓰지 않아도 되는 부분이 되었지만, JavaScript 라는 언어의 특성을 가장 잘 보여주는 특성 중 하나이기에 정리했습니다.

### 정의

hoist 라는 단어의 사전적 정의는 끌어올리기 라는 뜻이다. 자바스크립트에서 끌어올려지는 것은 변수이다. var keyword 로 선언된 모든 변수 선언은 호이스트 된다. 호이스트란 변수의 정의가 그 범위에 따라 선언과 할당으로 분리되는 것을 의미한다. 즉, 변수가 함수 내에서 정의되었을 경우, 선언이 함수의 최상위로, 함수 바깥에서 정의되었을 경우, 전역 컨텍스트의 최상위로 변경이 된다.

우선, 선언(Declaration)과 할당(Assignment)을 이해해야 한다. 끌어올려지는 것은 선언이다.

```javascript
function getX() {
  console.log(x); // undefined
  var x = 100;
  console.log(x); // 100
}
getX();
```
다른 언어의 경우엔, 변수 x 를 선언하지 않고 출력하려 한다면 오류를 발생할 것이다. 하지만 자바스크립트에서는 undefined라고 하고 넘어간다. var x = 100; 이 구문에서 var x;를 호이스트하기 때문이다. 즉, 작동 순서에 맞게 코드를 재구성하면 다음과 같다.

```javascript
function getX() {
  var x;
  console.log(x);
  x = 100;
  console.log(x);
}
getX();
```
선언문은 항시 자바스크립트 엔진 구동시 가장 최우선으로 해석하므로 호이스팅 되고, 할당 구문은 런타임 과정에서 이루어지기 때문에 호이스팅 되지 않는다.

함수가 자신이 위치한 코드에 상관없이 함수 선언문 형태로 정의한 함수의 유효범위는 전체 코드의 맨 처음부터 시작한다. 함수 선언이 함수 실행 부분보다 뒤에 있더라도 자바스크립트 엔진이 함수 선언을 끌어올리는 것을 의미한다. 함수 호이스팅은 함수를 끌어올리지만 변수의 값은 끌어올리지 않는다.

```javascript
foo( );
function foo( ){
  console.log('hello');
};
// console> hello
```
foo 함수에 대한 선언을 호이스팅하여 global 객체에 등록시키기 때문에 hello가 제대로 출력된다.

```javascript
foo( );
var foo = function( ) {
  console.log('hello');
};
// console> Uncaught TypeError: foo is not a function
```
이 두번째 예제의 함수 표현은 함수 리터럴을 할당하는 구조이기 때문에 호이스팅 되지 않으며 그렇기 때문에 런타임 환경에서 Type Error를 발생시킨다.

[참고링크][Hoisting]

[Hoisting]: https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/JavaScript#hoisting "Hoisting"