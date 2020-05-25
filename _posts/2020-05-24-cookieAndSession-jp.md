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

### クッキーとセッションを使用する理由

### 1 - HTTP プロトコルの特徴であり弱点を補うために使われる。

Connectionlessプロトコル(非連結志向)

クライアントがサーバにリクエスト(Request)をしたとき、
その要請に合う応答(Response)を送った後、接続を切る処理方式である。

HTTP 1.1 バージョンで接続を維持し、再活用する機能がDefault に追加された。
(keep-alive値に変更可能)

### 2 - Statelessプロトコル(ステータスを保持しない)

クライアントの状態情報を持たないサーバ処理方式である。

クライアントと最初の通信でデータをやり取りしたとしても、
2回目の通信で以前のデータを維持しない。

### But、実際にはデータの維持が必要な場合が多い。

情報が維持されなければ、毎回ページを移動するたびにログインしなおしたり、
商品を選んだのに購入ページで選択した商品の情報がなかったり、といったことが起こり得る。

よって、Stateful の場合に対処するためにクッキーとセッションを使用する。

クッキーとセッションの違いは、大きくステータス情報の保存位置である。

クッキーは"クライアント(=ローカルPC)"に保存し、セッションは"サーバー"に保存する。

サーバとクライアントが通信をするとき、通信が連続的につながらず、一度通信されると、途切れる。

したがって、サーバはクライアントが誰なのかを継続して認証しなければならない。 しかし、それは非常に煩わしいことだ。

また、ウェブページのロードを遅らせる要因にもなっている。 そんなわずらわしさを解決する方法がクッキーとセッションだ。

整理すると、クライアントと情報維持をするために使用するのがクッキーとセッションである。

### セッションを書けばいいんだけど、 敢えてクッキーを使う理由?

セッションがクッキーに比べてセキュリティも高い方ですが、クッキーを使用する理由は
セッションはサーバーに保存され、サーバー資源を使用するため、ユーザーが多いと消耗される資源が相当する。

このような資源管理の観点からクッキーとセッションを適切な要素と機能に並行して使用し、
サーバー資源の無駄遣いを防ぎ、ウェブサイトの速度を高めることができる。

### クッキー (Cookie)

HTTPの一種で、ユーザーが何らかのウェブサイトにアクセスする場合、
そのサイトが使用しているサーバから使用者のコンピュータに保存する小さな記録情報ファイルである。

HTTP でクライアントの状態情報をクライアントのPC に保存した。街
必要な時、情報を参照したり再使用したりすることができる。

### Cookieの動作手順

a. クライアントがページを要請する。 (ユーザーがウェブサイトにアクセス)

b.ウェブサーバはクッキーを生成する。

c. 作成したクッキーに情報を盛り込み、HTTP画面を返す時、一緒にクライアントに返す。

d. 渡されたクッキーはクライアントが持っていながら(ローカルPCに保存)、再びサーバーに要請する時、要請と共にクッキーを転送する。

e. 同一サイト再訪問時、クライアントのPCに該当クッキーがある場合は、リクエストページと一緒にクッキーを転送する。

### セッション(Session)

一定時間、同じユーザ(ブラウザ)から入ってくる
一連の要求を一つの状態と見て、その状態を一定に保つ技術である。

ここで一定時間、訪問者がウェブブラウザを通じて
ウェブサーバに接続した時点からウェブブラウザを終了して接続を終える時点をいう。

つまり、訪問者がウェブサーバーに接続している状態を一つの単位で見て、それをセッションという。

### セッションの動作手順

a. クライアントがページを要請する。 (ユーザーがウェブサイトにアクセス)

b. サーバは、アクセスしたクライアントのRequest-HeaderフィールドであるCookieを確認し、クライアントが該当session-idを送ったかを確認する。

c. session-idが存在しない場合、サーバは session-id を生成してクライアントに返す。

d. サーバからクライアントに返したsession-idをクッキーを使ってサーバに保存する。

クッキーの名前:JSESSIONID

e. クライアントは再接続時、このクッキー(JSESSIONID)を利用してsession-id値をサーバに伝達

||Cookie|Session|
|---|---|---|
|保存位置|クライアント(=接続者PC)|ウェブサーバ|
|保存形式|text|Object|
|満了時点|クッキー保存時の設定(ブラウザが終了しても、満了時点を過ぎないと自動削除されない)|ブラウザ終了時に削除(期間指定可能)|
|リソース|クライアント·リソース|ウェブサーバリソース|
|容量制限|計300個/1つのドメインあたり20個/1つのクッキー当たり4KB(=4096byte)|サーバーが許可する限り容量制限なし|
|速度|セッションより速い|クッキーより遅い|
|セキュリティ|セッションより悪い|クッキーより良い|

<br>

[参照][cookie]

[cookie]: https://hahahoho5915.tistory.com/32 "cookie"