---
layout: post
categories: "Java"
title: "Java Comfile"
author: "TY_K"
---

### Javaコンパイル過程

#### 入る前に
> JavaはOSに独立した特徴を持っています。 それが可能な理由は、JVM(Java Vitual Machine)のおかげです。 それでは、JVM(Java Vitual Machine)のどのような機能のために、OSを独立に実行させることができるのか、Javaコンパイルで見ていくことにしましょう。

![JavaComfile](https://user-images.githubusercontent.com/20508342/83427572-e776cd00-a46b-11ea-85bb-498c1eda7205.jpeg)

#### Javaコンパイル手順

[1] 開発者がJavaソースコード(.java)を作成します。

[2] Javaコンパイラ(Java Compiler)がJavaソースファイルをコンパイルします。 この時出てくるファイルはJavaバイトコード(.class)ファイルで、まだコンピュータが読めない-Java仮想マシンが理解できるコードです。 バイトコードの各コマンドは、1バイトの大きさのOpcodeと追加被演算子から成り立っています。

[3] コンパイルされたバイクコードをJVMのクラスローダーに伝達します。

[4] クラスローダーは、動的ローディング(Dynamic Loading)を通じて必要なクラスをローディング及びリンクし、ランタイムデータ領域(Runtime Dataarea)、すなわち、JVMのメモリにアップロードします。

- クラスローダーの細部の動作

    a.ロード:クラスファイルを読み込み、JVMのメモリにロードします。

    b.検証:Java言語明細(Java Language Specification)及びJVM明細に明示された通り構成されているか検査します。

    c.準備:クラスが必要とするメモリを割り当てます。 (フィールド、メソッド、インターフェースなど)

    d.分析:クラスの定数プール内のすべてのシンボリックリファレンスをダイレクトリファレンスに変更します。

    e.初期化:クラス変数を適切な値に初期化します。 (staticフィールド)

[5] 実行エンジン(Execution Engine)は、JVMメモリにアップロードされたバイクコードをコマンド単位で1つずつ読み込んで実行します。 この時、実行エンジンは2つの方式に変更します。

[i] インタプリタ:バイトコードコマンドを1つずつ読み込んで解析し実行します。 一つ一つの実行は早いものの、全体的な実行速度が遅いという短所を持っています。

[ii] JITコンパイラ(Just-In-Time Compiler):インタプリタのデメリットを補うために導入された方式で、バイトコード全体をコンパイルしてバイナリコードに変更し、以後はそのメソッドをこれ以上インタプリティングせずに、バイナリコードで直接実行する方式です。 一つずつインタプリティングして実行するのではなく、バイトコード全体がコンパイルされたバイナリコードを実行するので、全体的な実行速度はインタプリティング方式より速いです。

[参照][JavaComfile]

[JavaComfile]: https://github.com/gyoogle/tech-interview-for-developer/blob/master/Language/%5Bjava%5D%20%EC%9E%90%EB%B0%94%20%EC%BB%B4%ED%8C%8C%EC%9D%BC%20%EA%B3%BC%EC%A0%95.md#%EC%9E%90%EB%B0%94-%EC%BB%B4%ED%8C%8C%EC%9D%BC%EA%B3%BC%EC%A0%95 "JavaComfile"