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

### 1.Method area(メソッド領域)

クラスメンバー変数の名前、データタイプ、アクセス制御者情報などのフィールド情報やメソッドの名前、リターンタイプ、パラメータ、アクセス制御者情報などのメソッド情報、Type情報(Interfaceかclassか)、Constant Pool(定数プール：文字定数、タイプ、フィールド、オブジェクト参照が保存される)、static変数、finalclass変数などが生成される領域である。

### 2.Heap area (ヒップエリア)

newキーワードで生成されたオブジェクトと配列が作成される領域。

メソッド領域にロードされたクラスのみ生成でき、Garbage Collectorが参照されないメモリを確認して除去する領域である。

### 3.Stack area (スタックエリア)

地域変数、パラメータ、リターン値、演算に使用される仮値などが生成される領域である。

int a=10; というソースを作成したら、整数値が割り当てられるメモリ空間をaと決め、そのメモリ領域に値が10が入る。 つまり、スタックにメモリーに名前が「ａ」とつけ、値が１０のメモリー空間を作る。

クラス Person p = new Person(); というソースを作成したなら、Personpはスタック領域に生成され、newで生成されたPersonクラスのインスタンスはヒップ領域に生成される。

そして、スタック領域に生成されたpの値でヒップ領域のアドレス値を持っている。 すなわち、スタック領域に生成されたp がヒップ領域に生成されたオブジェクトを指して(参照して)いることである。

メソッドを呼び出すたびに個別にスタックが生成される

### 4.PC Register(PCレジスタ)

Thread（スレッド）が生成されるたびに生成される領域で、ProgramCounter、つまり現在のスレッドが実行される部分のアドレスとコマンドを保存している領域。 （*CPUのレジスタと異なる）

これを使ってスレッドを回って遂行できるようにする

### 5.Native method stack

Java以外の言語で作成されたネイティブコードのためのメモリ領域。

通常はC/C++などのコードを実行するためのスタック。 (JNI)


スレッドが生成された時を基準に、

1、2番のメソッド領域とヒップ領域をすべてのスレッドが共有し、

3、4、5 番のスタック領域とPC レジスタ、Native method stack は、それぞれのスレッド毎に生成され共有されない。


[参照][JavaMemory]

[JavaMemory]: https://jeong-pro.tistory.com/148 "JavaMemory"