---
layout: post
categories: "JS"
title: "Closure"
author: "TY_K"
---

### Closure(クローザー)とは?
MDN ではクロージャを次のように定義している

> クローザーは独立した(自由)変数を指す関数である。 または、クローザーの中に定義された関数は作られた環境を"記憶する"。

よく関数内で関数を定義して使用するとクローザーと呼ばれる。 しかし、大抵は定義した関数をリターンし、使用は外で行うことになる。 言葉で説明すると複雑なので、まずコードを見てみよう。

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

上記で定義したgetClosure()は関数を返し、返された関数はgetClosure()内部で宣言された変数を参照している。 また、このように参照された変数は関数実行が終わったからといって消えず、依然としてきちんとした値を返していることがわかる。

ここで返された関数がクローザーであるが、MDN で定義された内容でも述べたように環境を記憶しているように見える。 まだよく理解できないので、他の例題も一度見てみる。

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