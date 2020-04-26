---
layout: post
categories: "DataStructure"
title: "Binary Tree"
author: "TY_K"
---

### Tree

ツリーはスタックやキューのような線形構造ではなく、非線形資料構造である。 
ツリーは階層的関係(Hierarchical Relationship)を表現する資料構造である。 このツリーという資料構造は表現に集中する。 
何かを保存して取り出すべきだという考え方から脱し、ツリーという資料構造を眺めてみよう。

ツリーを構成している構成要素(用語)

* Node (ノード):ツリーを構成しているそれぞれの要素を意味する。
* Edge (幹線):ツリーを構成するためにノードとノードを結ぶ線を意味する。
* Root Node (ルートノード):ツリー構造で最上位にあるノードを意味する。
* Terminal Node (= leaf Node、端末ノード): 下位に他のノードが接続されていないノードを意味する。
* Internal Node (内部ノード、 タマールノード): 端末ノードを除いた全てのノードでルートノードを含む。

### Binary Tree(二進ツリ)

ルートノードを中心に二つのサブツリー(大きなツリーに属する小さなツリー)に分かれる。

また、分かれた二つのサブツリーもすべてバイナリでなければならない。

ツリーでは各フロアごとに数字を付けてこれをツリーのレベル(レベル)という。

レベルの値は0 から始めて、したがってルートノートのレベルは0 である。

そしてツリーの最高レベルを指して該当ツリーのheight(高さ)という。

### Perfect Binary Tree、Complete Binary Tree、Full Binary Tree

すべてのレベルがぎっしり詰まった二進ツリーのことをPerfect Binary Treeという。 

上から下に、左から右に順にぎっしりと詰まっている二進ツリーのことをComplete Binary Treeという。 

すべてのノードが0個あるいは2個の子ノードだけを持つ二進ツリーを指してFull Binary Treeという。

[参照][DataStructure]

[DataStructure]: https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/DataStructure#tree "DataStructure"