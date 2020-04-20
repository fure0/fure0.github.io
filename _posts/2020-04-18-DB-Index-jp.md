---
layout: post
categories: "DB"
title: "DB-INDEX"
author: "TY_K"
---

## DB-インデックス(INDEX)とは？

DBを使うと、ユーザが望む内容を迅速に提供するため悩むことがあります。

### 1."どうすれば素早く情報を検索、提供してくれるか?"-迅速なサービス提供

### 2."できるだけ少ない資源で同一のサービスを提供できるか"-効率的なサービス運営

この二つの質問をたくさん思い浮かべると思います。

2番の質問は、クエリ最適化、キャッシングの利用と同じローレベルの運営部分です。

ここでは1番の質問を解決できるインデックスについてお話したいと思います。

### 本とDBを比較してみましょう。

> 目次 - インデックス

> 内容 - データ

必要な本の内容を探すときは、まず目次をみて、必要な本の内容のあるページ番号で本の内容を探します。

同じように、

必要なデータを探すときは、まずインデックスで該当データの位置アドレスを探し、位置アドレスで該当するデータを探します。

目次とインデックスの共通点は見つけやすいようにするため、あらかじめ探そうとするデータを整列して持っていることです。

あらかじめ整列して持っているから探す時 a、 b、 c、 d、 ... または、あ、い、う、え…のように整列された順番に素早く検索できます。

### DBでインデックスとデータ資料の構造

- インデックスの資料構造 : Sorted List

> Sorted Listは、保存される値を常に整列した状態に維持する資料構造です。

- データの資料構造 : ArrayList

> Array Listは、保存される順序に従って、整列なしで保存される値を保持する資料構造です。

### インデックスの資料構造 Sorted Listの長所、短所

#### 長所
すでに整列されているので、DBでSELECTクエリを使用する場合、非常に速いスピードで必要な結果を検索できます。

#### 短所
データが変化するINSERT、UPDATE、DELETEクエリを使用する場合、ソートしてデータを保存しなければならないため、クエリの遂行時間が長くなります。

*すなわち、インデックスの使用理由は、データを生成(INSERT)、変化(UPDATE)、削除(DELETE)性能は低下しますが、データの読み取り検索(SELECT)の性能を向上させるためです。

ですから、一つのテーブルであまりにも多くのカラムにインデックスを生成すると、データ保存性能が落ちてしまいます。

### インデックスの役割

1.Primary Key

- テーブルのひとつのレコードを代表するカラム値で作られたインデックスです。

- このカラム値は、一つのレコードを識別できる識別子と呼ばれます。

- NULLは使用できません。

- 重複を許しません。

2.Secondary Key

- Primary Keyを除くすべてのインデックスをSecondary Indexといいます。

- 補助インデックスキーとも呼ばれます。

### インデックスのデータ保存方式

1.B-Tree Index

- 最も一般的に使用されるインデックスアルゴリズムです。

- 最も古いアルゴリズムで安定した状態の保存方法です。

- カラム値を変形せず、元の値を基準に用いてインデックスするアルゴリズムです。

2.Hash Index

- カラム値をハッシュ値で計算してインデックスするアルゴリズムです。

- 非常に速い速度に対応しています。

- カラム値がハッシュ値に変形してインデックスに活用されるため、カラム値の一部分を検索する場合、不可能です。

- 主にメモリ基盤のデータベースで多く使われます。 理由は、メモリアドレス形態でその値をすべて参照する形で使用するからです。

3.Fractal-Tree Index

- B-Tree Index のデメリットを補うために考案されたアルゴリズムです。

- B-Treeのようにカラム値を変形せずにインデックスします。

- データが保存されたり削除される時、処理するコストを削減できるように設計されており、B-Treeより経済的です。

ただ、まだ成熟していないインデックスアルゴリズムです。

### インデックスのデータの重複許容可否

:レコードのインデックス値が重複して使用する場合と、重複せずに固有値を持っている場合に分けられます。

1.重複不可能なインデックス(Unique Index)

2.重複可能なインデックス(Non-Unique Index)

上記2つのインデックス区分は、MySQLオプティマイズが検索を処理する際に、相当なロジックの違いを与えます。 つまり、速度の差で現れます。

[参照][INDEX]

[INDEX]: https://interconnection.tistory.com/97 "INDEX"