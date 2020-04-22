---
layout: post
categories: "web"
title: "RESTful API"
author: "TY_K"
---


### RESTful

> ワールドワイドウェブ(World Wide Web a.k.a WWW)のような分散ハイパーメディアシステムのためのソフトウェアアーキテクチャの一形式でリソースを定義し、リソースに対するアドレスを指定する方法全般に対するパターン

RESTとは、REpresentational State Transferの略である。 これに"~ful"という形容詞形語尾をつけて"~なAPI"という表現で使われる。 つまり、REST の基本原則を誠実に守ったサービスデザインは"RESTful"と表現できる。

REST がデザインパターンである、アーキテクチャである多くの物語が存在するが、一つのアーキテクチャとして考えられる。 より正確な表現で言えば、RESTはResource Oriented Architectureである。 API 設計の中心にリソース(Resource)があり、HTTP Method を通じてリソースを処理するように設計することである。

### REST 6つの原則

* Uniform Interface
* Stateless
* Caching
* Client-Server
* Hierarchical system
* Code on demand

cf)より詳しい内容についてはReferenceをご参照ください。

### RESTfulにAPIをデザインするということは何を意味するのか。(要約)

> リソースと行為を明示的かつ直感的に分離する。

* リソースはURIで表現されるが、リソースが指すものは名詞で表現されなければならない。
* 行為は、HTTP Method で表現し、GET(照会)、POST(生成)、PUT(既存entity 全体修正)、PATCH(既存entity 一部修正)、DELETE(削除)を明らかな目的で使用する。

> MessageはHeaderとBodyを明確に分離して使用する。

* Entity についての内容はbody に盛り込む。
* アプリケーションサーバが行動する判断の根拠となるコントロール情報であるAPI バージョン情報、応答を受けようとするMIME タイプなどはheader に収める。
* header と body はhttp header と http body に分けることもでき、http body に入る json 構造に分離することもできる。

> API版を管理する。

* 環境は常に変わるのでAPI のデザインが変更されることもあることに留意しよう。
* 特定のAPI を変更する際には、必ず下位互換性を保障しなければならない。

> サーバとクライアントが同じ方式を使用して要請するようにする。

* ブラウザは form-data 形式の submit で送り、サーバでは json 形式に送るという分離よりは、json に送るか、両方とも form-data 形式に送るか、一つに統一する。
* 別の言葉で表現すると、URI がプラットフォーム中立的でなければならない。

### どのようなメリットが存在するのか?
1. Open API が提供しやすい
2. マルチプラットフォームの支援や連動が容易である。
3. 希望するタイプでデータをやり取りできる。
4. 既存のWebインフラ(HTTP)をそのまま使用できる。

### 短所は何があるかな?
1. 使えるメソッドが4つしかない。
2. 分散環境には不向きである。
3. HTTP 通信モデルに対してのみサポートする。

[参照][RESTful]

[RESTful]: https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/Development_common_sense#restful-api "RESTful"