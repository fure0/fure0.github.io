---
layout: post
categories: "Java"
title: "JVM & Java Memory Area"
author: "TY_K"
---

## JVM(Java Virtual Machine)

Java仮想マシンでJavaバイトコードを実行できる主体である。

CPU やオペレーティングシステム（プラットフォーム）の種類と関係なく実行が可能である。

すなわち、オペレーティングシステムの上で動作するプロセスで、Javaコードをコンパイルして取得したバイトコードを当該オペレーティングシステムが理解できる機械語に置き換えて実行させる役割を果たす。

JVMの構成を見ると、大きく4つ（Class Loader, Execution Engine, Garbage Collector, Runtime Data Area）に分けられる。

![JVM](https://user-images.githubusercontent.com/20508342/86537186-17f8cd80-bf28-11ea-9c63-1ed68118bd66.png)

### 1.Class Loader

Javaでソースを作成すると、Person.javaのように.javaファイルが生成される。

.javaソースをJavaコンパイラがコンパイルすると、Person.classのような.classファイル(バイトコード)が生成される。

このように生成されたクラスファイルを組み合わせてJVMがOSから割り当てられたメモリ領域であるRuntime Data Areaに積載する役割をClass Loaderが担う。(Javaアプリケーションが実行中のとき、このような作業が遂行される。)

### 2.Execution Engine

Class Loader によってメモリに積載されたクラス（バイトコード）を機械語に変更し、コマンド単位で実行する役割を果たす。

コマンドを一つ一つ実行するインタプリタ方式があり、JIT(Just-In-Time)コンパイラを用いる方式がある。

JIT コンパイラは、適切な時間に全バイトコードをネイティブコードに変更し、Execution Engine がネイティブにコンパイルされたコードを実行することで性能を高める方式である。

### 3.Garbage Collector

Garbage Collector(GC)は、Heapメモリ領域に生成(積載)されたオブジェクトの中から参照されないオブジェクトを探索し、除去する役割を担う。

ＧＣが役割を果たす時間は正確にいつなのか分からない。 (参照がなくなり、解除されることを保障しない)

もう一つの特徴は、GCが行われている間、GCを遂行するスレッドではない他のすべてのスレッドが一時停止される。

特にFull GCが起きて数秒間すべてのスレッドが停止すると、障害につながる致命的な問題が生じる可能性があるの。 （GCに関する内容は、以下のHeap領域メモリを説明する際にさらに詳しく調べる。）

### 4.Runtime Data Area

JVMのメモリ領域で、Javaアプリケーションを実行するときに使用されるデータを積載する領域。

この領域は大きくMethod Area、Heap Area、Stack Area、PC Register、Native Method Stackに分けられる。

## Javanタイムメモリ(Runtime Data area)構造

![RuntimeDataArea](https://user-images.githubusercontent.com/20508342/86537207-38288c80-bf28-11ea-89dc-ab448202d7e4.png)

