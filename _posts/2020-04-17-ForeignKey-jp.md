---
layout: post
categories: "DB"
title: "Primary Key And Foreign Key"
author: "TY_K"
---

## [Database]基本鍵(Primary Key)、外来鍵(Foreign Key)とは??

* PKとFKはテーブルの必須要素として、すべてのテーブルはこれらのうち一つ以上を必ず含めなければならない。
* DBでテーブルを生成する際、1つまたはそれ以上の項目を基本キー(PK)に設定できる。
* PK = Not null + Unique

## Primary Key
* 基本鍵は他の項目と絶対に重複して表せない単一値(unique)を持つ。
* 基本鍵は絶対null(何の値もない状態)の値が持てない。
  (車の番号のような概念)
* 一つ以上のカラムがグループ化し、基本キーとしても使える。
* 固有インデックスが自動で生成される
* テーブルは基本キーを1つまで持つことができる。
* 技術的側面で、基本鍵は単一値(unique)とnot Nullであれば機能的に等しく動作するが、
実際に基本キーのように区分されるのはただ一つ、つまりすべてUniqueとnot Nullが基本キーになるのではない。

## Foreign Key
* テーブルを生成するとき、FKを定義する。
* FKが定義されたテーブルは子テーブルである。
* 参照されるテーブルを親テーブルという。
* 親テーブルは予め生成されていなければならない。
* 親テーブルの参照されるコラムに存在する値のみを入力できる。
* 親テーブルはFKにより削除できない。

[参照][PKFK]

[PKFK]: http://itnovice1.blogspot.com/2019/01/database-primary-key.html "PKFK"