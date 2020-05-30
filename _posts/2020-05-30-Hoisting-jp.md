---
layout: post
categories: "JS"
title: "Hoisting"
author: "TY_K"
---

ES6文法が標準化されるにつれ、それほど気にしなくていい部分になりましたが、JavaScriptという言語の特性を最もよく表している特性の一つなのでまとめました。

### 定義

"hoist"という単語の辞書的定義は"引き上げ"という意味だ。 JavaScriptから引き上げられるのは変数である。 var keyword で宣言されたすべての変数宣言はホイストされる。 ホイストとは変数の定義がその範囲によって宣言と割り当てに分離することを意味する。 すなわち、変数が関数内で定義された場合、宣言が関数の最上位で、関数の外で定義された場合、全域コンテキストの最上位に変更となる。

まず、宣言(Declaration)と割り当て(Assignment)を理解しなければならない。 引き上げられるのは宣言だ。

```javascript
function getX() {
  console.log(x); // undefined
  var x = 100;
  console.log(x); // 100
}
getX();
```
他の言語の場合、変数x を宣言せずに出力しようとするとエラーが生じるだろう。 しかし、JavaScriptではundefinedと呼ぶ。 var x=100; この構文でvar x;をホイストするからである。 すなわち、動作手順に合わせてコードを再構成すると次のようになる。

```javascript
function getX() {
  var x;
  console.log(x);
  x = 100;
  console.log(x);
}
getX();
```
宣言文は常にJavaScriptエンジン駆動時に最優先に解釈するのでホイスティングされ、割り当て構文はランタイムの過程で行われるためホイスティングされない。

関数が自分の位置するコードに関わらず、関数宣言文の形で定義した関数の有効範囲は、全コードの最初から始める。 関数宣言が関数実行部分より後ろにいても、JavaScriptエンジンが関数宣言を引き上げることを意味する。 関数ホイスティングは関数を引き上げるが、変数の値は引き上げない。

```javascript
foo( );
function foo( ){
  console.log('hello');
};
// console> hello
```
foo 関数に対する宣言をホイスティングしてglobal オブジェクトに登録させるため、hello がきちんと出力される。

```javascript
foo( );
var foo = function( ) {
  console.log('hello');
};
// console> Uncaught TypeError: foo is not a function
```
この2番目の例題の関数表現は関数リテラルを割り当てる構造であるためホイスティングされず、そのためランタイム環境でType Errorを発生させる。

[参照][Hoisting]

[Hoisting]: https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/JavaScript#hoisting "Hoisting"