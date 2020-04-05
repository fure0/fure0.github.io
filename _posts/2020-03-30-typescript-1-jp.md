---
layout: post
categories: "JS"
title: "Type Script"
author: "TY_K"
---

<style>
    .post img {
        margin : 0
    }
</style>


# TypeScriptとは?

TYPEがある javascriptを作成し、普通のjavascriptにコンファイルする。
Any browser、Any host、Any Os、Open source
便利な機能、ms開発。

以下TypeScriptはtsに名称

# なぜtsを使うべきか？
タイプが指定出来ることで作成しながらミスをする可能性が減る。
コードでデータの予測が出来る。

# 簡単に作成して見よう。

# ts 設置

npm install -g typescript
<img width="569" alt="install" src="https://user-images.githubusercontent.com/20508342/77920750-d5e13f80-72d9-11ea-9a3f-89ff26f6570a.png"> 
3.3.3バージョンが設置された事を確認。


テストコード greeter.ts を作成。
<img width="367" alt="greeter" src="https://user-images.githubusercontent.com/20508342/77921001-26589d00-72da-11ea-89a8-bb61efc5d8d3.png">

tscコマンドで tsにコンファイル。
<img width="397" alt="comfile" src="https://user-images.githubusercontent.com/20508342/77921192-691a7500-72da-11ea-8cd9-839c62333df6.png">
greeter.jsにコンファイルされた事を確認。
ここまでは変更点がない 普通のjsと同じ。

ここからtsの機能を使って見ると
<img width="367" alt="greeter" src="https://user-images.githubusercontent.com/20508342/77921344-9cf59a80-72da-11ea-89d2-eb2a17b0026d.png">

greeter functionの 引数 string タイプを追加
<img width="797" alt="cngValue" src="https://user-images.githubusercontent.com/20508342/77921479-c9111b80-72da-11ea-9a19-70f5c4e72fa8.png">
userを arrayに変更すると, [ts] ‘number[]’ 形式の引数は ‘string’形式の 媒介変数に 与えられません。
とのエラーメッセージを確認。

<img width="631" alt="comfileError" src="https://user-images.githubusercontent.com/20508342/77921563-e5ad5380-72da-11ea-9907-936c9bc1036a.png">
IDEでもエラーを確認出来るが、コンファイル時もエラーが出力される事を確認

<img width="320" alt="error1" src="https://user-images.githubusercontent.com/20508342/77921638-fcec4100-72da-11ea-9b0d-08fb4b5dba9c.png">
同じく引数がない場合も適切なエラーを出力

interfaceの場合
<img width="539" alt="interfaceTs" src="https://user-images.githubusercontent.com/20508342/77921751-23aa7780-72db-11ea-9483-ebe530c55ea5.png">

interfaceを作成後コンファイル時
<img width="538" alt="interfaceJs" src="https://user-images.githubusercontent.com/20508342/77921806-30c76680-72db-11ea-97dc-329d72a461b6.png">
適切にコンファイルされた事を確認

classの場合も
<img width="809" alt="classTs" src="https://user-images.githubusercontent.com/20508342/77921907-4d639e80-72db-11ea-9379-8c97097afeef.png">

適切にコンファイルされた事を確認。
<img width="618" alt="classJs" src="https://user-images.githubusercontent.com/20508342/77921945-5b192400-72db-11ea-8f7d-5591d95ab52a.png">

最後にhtmlにtsからjsにコンファイルされたファイルが上手く動作するか確認して見る。
<img width="434" alt="greeterHtml" src="https://user-images.githubusercontent.com/20508342/77922021-771cc580-72db-11ea-90e1-61625488d0a0.png">

tsに作成したjsが問題なく動作する事を確認
<img width="470" alt="greeterHtml2" src="https://user-images.githubusercontent.com/20508342/77922075-86037800-72db-11ea-9f5e-bc562bf98c3b.png">
