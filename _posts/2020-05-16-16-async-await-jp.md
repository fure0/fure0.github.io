---
layout: post
categories: "JS"
title: "async/await"
author: "TY_K"
---

### async & await

asyncとawaitはJavaScriptの非同期処理パターンのうち、最近出た文法です。 従来の非同期処理方式であるコールバック関数とプロミスの短所を補完し、開発者が読みやすいコードを作成できるようにサポートします。

### async & await プレビュー

簡単関数に

```javascript
function logName() {
  var user = fetchUser('domain.com/users/1');
  if (user.id === 1) {
    console.log(user.name);
  }
}
```

以下のように asyncとawaitを入れると

```javascript
async function logName() {
  var user = await fetchUser('domain.com/users/1');
  if (user.id === 1) {
    console.log(user.name);
  }
}
```

これで async awaitコードになります。

### async & await 適用されたコードとそうでないコード

さて、先ほど見たコードとはどのような意味なのか、一度見ていきましょう。 まず、先ほど見たlogName()関数コードをもう一度見てみましょう。

```javascript
function logName() {
  var user = fetchUser('domain.com/users/1');
  if (user.id === 1) {
    console.log(user.name);
  }
}
```
ここでfetchUser()というコードはサーバからデータを受け取ってくるHTTP通信コードだと仮定しました。 
一般的にJavaScriptの非同期処理コードは以下のようにコールバックを使用することで、コードの実行順序が保証されます。
```javascript
function logName() {
  // 下のuser変数は、上記のコードと比較するためにわざわざ残しておきました。
  var user = fetchUser('domain.com/users/1', function(user) {
    if (user.id === 1) {
      console.log(user.name);
    }
  });
}
```
すでに上のように、コールバックで非同期処理コードを作成することに慣れている方なら問題ありませんが、この考え方に慣れていない方は首をかしげるでしょう。

それで、私たちが初めてプログラミングを学んだその時その事故に戻るのです。 以下のように簡単に考えましょう。

```javascript
// 非同期処理をコールバックにしなくても良いなら
function logName() {
  var user = fetchUser('domain.com/users/1');
  if (user.id === 1) {
    console.log(user.name);
  }
}
```
サーバーからユーザデータを読み込んで変数に保存し、ユーザIDが1ならユーザ名を出力する。

こうするにはasync awaitだけ付ければいいです :)
```javascript
// async + await適用後
async function logName() {
  var user = await fetchUser('domain.com/users/1');
  if (user.id === 1) {
    console.log(user.name);
  }
}
```

### async & await 基本文法

```javascript
async function test() {
  await method();
}
```

まず、関数の前にasyncという予約語をつけます。 それから関数の内部ロジック中にHTTP通信をする非同期処理コードの前にawaitを付けます。 ここで注意すべき点は、非同期処理メソッドが必ずプロミスオブジェクトを返してこそ、awaitが意図した通りに動作します。

一般に、awaitの対象となる非同期処理コードは、Axiosなどのプロミスを返すAPI呼び出し関数です。

### async & await 簡単な例題

```javascript
function fetchItems() {
  return new Promise(function(resolve, reject) {
    var items = [1,2,3];
    resolve(items)
  });
}

async function logItems() {
  var resultItems = await fetchItems();
  console.log(resultItems); // [1,2,3]
}
```
まず、fetchItems()関数は、プロミスオブジェクトを返す関数です。 プロミスは"JavaScript非同期処理のためのオブジェクト"と習っていました。 fetchItems()関数を実行すると、プロミスが移行(Resolved)され、結果値はitems配列されます。

そしてlogItems()関数を見てみましょう。 logItems()関数を実行すると、fetchItems()関数の結果値であるitems配列がresultItems変数に含まれます。 したがって、コンソールには[1、2、3]が出力されます。

awaitを使用しなかったら、データを受け取ってきた時点でコンソールを出力できるようにコールバック関数や.then()などを使用すべきだったでしょう。 しかし、async await文法のおかげで非同期に対する思考をしなくても良いのです。

※ 参考:もし上のコードがなぜ非同期処理コードなのかよく理解できなければ、fetch Items()を下記の関数に変えて実行しても構いません。

```javascript
// HTTP通信動作を模したコード
function fetchItems() {
  return new Promise(function(resolve, reject) {
    setTimeout(function() {
      var items = [1,2,3];
      resolve(items)
    }, 3000);
  });
}

// jQuery ajax コード
function fetchItems() {
  return new Promise(function(resolve, reject) {
    $.ajax('domain.com/items', function(response) {
      resolve(response);
    });
  });
}
```

### async & await 実用例題

async & await文法が最も輝く瞬間は、複数の非同期処理コードを扱う時です。 以下のようにそれぞれユーザーとすべきことリストを受け取ってくるHTTP通信コードがあるとします。

```javascript
function fetchUser() {
  var url = 'https://jsonplaceholder.typicode.com/users/1'
  return fetch(url).then(function(response) {
    return response.json();
  });
}

function fetchTodo() {
  var url = 'https://jsonplaceholder.typicode.com/todos/1';
  return fetch(url).then(function(response) {
    return response.json();
  });
}
```

これらの関数を実行すると、それぞれのユーザー情報とすべき情報が盛り込まれたプロミスオブジェクトが返されます。

では次に、この2つの関数を利用してすべき事題を出力してみましょう。 探ってみる例題コードのロジックは以下の通りです。

1. fetch User()を利用してユーザー情報を呼び出す
2. 受け取ったユーザーIDが1ならやることの情報を呼び出す
3. 受け取ったこと情報のタイトルをコンソールに出力

それではコードを見てみましょう。

```javascript
async function logTodoTitle() {
  var user = await fetchUser();
  if (user.id === 1) {
    var todo = await fetchTodo();
    console.log(todo.title); // delectus aut autem
  }
}
```
logTodoTitle()を実行すると、コンソールにdelectus autemが出力されるでしょう。 上記の非同期処理コードをもしコールバックやプロミスにしていたら、はるかにコードが長くなったでしょうし、インデンティングだけでなく可読性も良くなかったでしょう。 このように、asyncawait文法を用いると既存の非同期処理コード方式で思考しなくても良いという長所が生じます。

※参考:上記関数で使用したfetch()APIは、クロムのような最新ブラウザでのみ動作します。

### async & await例外処理

async&awaitで例外を処理する方法がtry catchです。 プロミスでエラー処理のために .catch()を使用したように、asyncではcatch{}を使用すればいいです。

先ほどコードにすぐにtry catch文法を適用してみましょう。
```javascript
async function logTodoTitle() {
  try {
    var user = await fetchUser();
    if (user.id === 1) {
      var todo = await fetchTodo();
      console.log(todo.title); // delectus aut autem
    }
  } catch (error) {
    console.log(error);
  }
}
```

上記のコードを実行中に発生したネットワークの通信エラーだけでなく、簡単なタイプエラーなどの一般的なエラーまでもキャッチできます。 発見されたエラーはerrorオブジェクトに含まれるので、エラーのタイプに合わせてエラーコードを処理してください。

[参照][async]

[async]: https://joshua1988.github.io/web-development/javascript/js-async-await/ "async"