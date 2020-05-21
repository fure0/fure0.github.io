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

出力された結果を見ると、text変数が動的に変化しているように見える。 実際は、textという変数自体が何度も生成されている。 すなわち、hello1()とhello2()、hello3()は異なる環境を持っている。

### クローザーによる秘匿化

一般的に、JavaScriptでオブジェクト指向プログラミングをいうとすれば、Prototypeを通じてオブジェクトを取り扱うことをいう。 ES2015にはクラスもあるけど、これを言うとあまりにも話が長くなるので、次にしよう。

いずれにせよ、Prototype を通じたオブジェクトを作る際の主な問題の一つは、Private variables へのアクセス権限問題である。 例題コードを見てみよう。

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

上からHello()で生成されたオブジェクトは全て_name という変数を持つことになる。 変数名の前にunderscore(_)を含むため、一般的なJavaScriptネーミングコンベンションを考えるとき、この変数はPrivate variable として使いたいという意図がわかる。 しかし、実際には依然として外部からも簡単にアプローチできる変数に過ぎない。

この場合、クローザーを使用して外部から変数に直接アクセスすることを制限することができる。

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

特にインターフェースを提供するものでなければ、ここでは外部から_name にアクセスする方法が全くない。 このように隠匿化も思ったより簡単に解決できる。

### loop文クローザー

磨り減った例題ではあるが、だからといって見落とすには惜しい例題なので、ここでも一度見極めなければならないだろう。
```javascript
var i;
for (i = 0; i < 10; i++) {
  setTimeout(function() {
    console.log(i);
  }, 100);
}
```

簡単に0-9までの整数を出力するコードだが、実際に回してみると、とんでもないことに10が10回出力されるのが見られる。 なぜだろう。

まずsetTimeout() に印字で渡した匿名関数はすべて0.1 秒後に呼び出されるだろう。 その0.1秒間にすでに繰り返し文が全て巡回され、i値は既に10になった状態。 その際、匿名関数が呼び出され、既に10 になってしまったi を参照するものである。

この場合も、クローザーを使えば好きなように動作させることができる。

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
途中でIIFE を付け加えてsetTimeout() にかかった匿名関数をクローザーにした。 先に述べたように、クローザーは作られた環境を覚えている。 このコードにおいて、i はIIFE 内にjという形で注入され、クローザーによってそれぞれ異なった環境の中に含まれる。 繰り返し文は10回繰り返されるので10個の環境が生じ、10個の異なる環境に10個の異なるjが生じる。

このへんで、IIFE媒介変数としてiを越えずに、そのまま直接参照してもいいのではないかという疑問を抱くかも知れない。 しかし、直接やってみると、望み通りに動作しない。

上記の例題ではIIFEを通じてクロージャ毎に環境が生じる。 しかし、因子にi を渡さなければ、当然クロージャが参照するIIFE の関数スコープでもi 値がないため、生成当時の外部スコープであるグローバルを探索することになり、結局すべて同じi を参照することになる。 反面、因子でi を超えると、IIFE で作った10個のスコープにすべてi という変数が異なる値で生じるので、正常に動作できるのである。

### クローザーの性能

クローザーは各自の環境を持つ。 この環境を記憶するためには、当然メモリが消耗されるだろう。 クローザーを生成しておいて参照を取り除かないのは、C++で動的割り当てでオブジェクトを生成しておいてdeleteを使用しないのと似ている。 クローザーによって内部変数を参照している間は、内部変数が占めるメモリをGCが回収しない。 したがって、クローザーの使用が終われば、参照を除去した方が良い。

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

// ここでメモリをreleaseさせるクローザーの参照を除去しなければならない。
hello1 = null;
hello2 = null;
hello3 = null;
```
このようにメモリ管理において弱点があるが、追加でスコープチェーンを検索する時間と新しいスコープを生成するのにかかるコストも考えざるを得ない。

[参照][closure]

[closure]: https://hyunseob.github.io/2016/08/30/javascript-closure/ "closure"
