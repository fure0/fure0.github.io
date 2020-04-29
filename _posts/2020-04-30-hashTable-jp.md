---
layout: post
categories: "DataStructure"
title: "Hash Table"
author: "TY_K"
---

### Hash Table(ハッシュテーブル)

> ハッシュテーブルは連関配列構造を利用してキー(key)に結果値(value)を保存する資料構造

### 連関配列構造(associative array)とは、

キー(key) 1個と値(value) 1個が1:1で関連している資料構造である。 したがってキー(key)を用いて値(value)を導き出すことができる。

連関配列構造は、select、insert、edit、delete命令を支援する。

(ハッシュテーブルでも同様に適用される。)

### ハッシュテーブルの構造(Hash Table Data Structure)

![HashTable](https://user-images.githubusercontent.com/20508342/80629159-6784ce00-8a8d-11ea-9f8d-de07af7cf745.png)

### ハッシュテーブルの要素

#### 1. キー(key)

固有の値であり、ハッシュ関数のinput となる。 様々な長さの値となり得る。 この状態で最終リポジトリに保存されると、多様な長さだけのリポジトリを構成しておかなければならないため、ハッシュ関数に値を変えて保存しなければ空間の効率性を追求できない。

#### 2. ハッシュ関数(Hash Function)

キー(key)をハッシュ(hash)に置き換える役割を持つ。 様々な長さのキー(key)を一定の長さを持つハッシュ(hash)に変更し、リポジトリを効率的に運用できるようにサポートする。 ただし、異なるキー(key)が同じハッシュ(hash)となる場合をハッシュ衝突(Hash Collision)というが、ハッシュ衝突を引き起こす確率を極力減らす関数を作ることが重要である。

#### 3. ハッシュ(Hash)

ハッシュ関数(Hash Function)の結果物であり、リポジトリ(bucket、 slot)で値(value)とマッチングして保存される。

#### 4. 値(Value)

リポジトリ(bucket、slot)に最終的に保存される値でキーとマッチングされ、保存、削除、検索、アクセスが可能でなければならない。