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

||Cookie|Session|
|---|---|---|
|保存位置|クライアント(=接続者PC)|ウェブサーバ|
|保存形式|text|Object|
|満了時点|クッキー保存時の設定(ブラウザが終了しても、満了時点を過ぎないと自動削除されない)|ブラウザ終了時に削除(期間指定可能)|
|リソース|クライアント·リソース|ウェブサーバリソース|
|容量制限|計300個/1つのドメインあたり20個/1つのクッキー当たり4KB(=4096byte)|サーバーが許可する限り容量制限なし|
|速度|セッションより速い|クッキーより遅い|
|セキュリティ|セッションより悪い|クッキーより良い|

