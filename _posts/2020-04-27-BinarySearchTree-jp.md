---
layout: post
categories: "DataStructure"
title: "Binary Search Tree"
author: "TY_K"
---

![Binary_search_tree](https://user-images.githubusercontent.com/20508342/80315123-981bfc00-8830-11ea-8183-2da15510e5d3.png){: width="60%" height="60%"}

### BST (Binary Search Tree)

効率的な探索のためには、どのように探せばいいかばかり悩んではならない。

それよりは効率的な探索のための保存方法が何なのかを悩まなければならない。

バイナリ探索ツリーはバイナリの一種である.

ただし、バイナリ探索ツリーにはデータを保存する規則がある。

そして、その規則は特定のデータの位置を探すのに使うことができる。

* ルール1 バイナリ探索ツリーのノードに保存されたキーは唯一である。
* ルール2 親のキーが左のノードより大きい。
* ルール3 親のキーが右ノードより小さい。
* ルール4 左と右サブツリーもバイナリ探索ツリーである。

二進探索ツリーの探索演算は O(logn) の時間複雑度を持つ。

実は正確に言えば"O(h)"と表現するのが正しい。

ツリーの高さを一つずつ加えるほど、追加できるノードの数が2倍ずつ増えるからだ。

しかし、このような二進探索ツリーは、Skewed Tree(偏向ツリー)となり得る。

保存順序に従って、一方のみノードが追加される場合が発生するからだ。

この場合、性能に影響を与えることになり、探索のWorst Caseとなり、時間の複雑度はO(n)になる。

配列より多くのメモリを使ってデータを保存したが、探索に必要な時間の複雑度が同じになる非効率的な状況が発生する。

これを解決するためにRebalancing 手法が登場した。

バランスをとるためのツリー構造の再調整をRebalancing という。

この手法を実装したツリーには複数の種類が存在するが、その中の1つがRed-BlackTreeである。

[参照][DataStructure]

[DataStructure]: https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/DataStructure#bst-binary-search-tree "DataStructure"