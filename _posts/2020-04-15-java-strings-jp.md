---
layout: post
categories: "Java"
title: "Java String"
author: "TY_K"
---

## JAVA String、 String Buffer、 String Builderの違い

先にこれらのクラスの共通点は、すべて<span style="color:blue">String(文字列)を保存·管理するクラス</span>

### String vs StringBuffer, StringBuilder

String は immutable(不変)で、String Buffer、String Builder は mutable(可変)である。

つまり、StringクラスはStringBufferクラスやStringBuilderクラスとは異なり、<span style="color:blue">リテラルによって生成されると、そのインスタンスのメモリ空間は絶対に変わらない。</span>
```java
String literalString = "literal"; //リテラルで生成する方式
String newString = new String("literal"); //newで生成する方式

//上で"literal"という文字列をString Poolで生成したため、以後追加したstr1、 str2、 str3は追加的に生成せずに同じ文字列を指す。

String str1 = "literal"; 
String str2 = "literal"; 
String str3 = "literal";
```

上記コードの2つのString class生成方式によって分けられるが、両方式ともJVMメモリのうちヒップ領域に生成される点は同じである。

しかし、リテラルで生成すると、特殊に<span style="color:blue">"String Pool"</span>という空間に生成される。 このメモリ空間に生成された<span style="color:blue">文字列の値</span>は絶対変わらない。


そのため、'+'演算や.concat()メソッドを利用して文字列の値に変化をつけてもメモリ空間内の値が変わるのではなく、"String Pool"という空間の中にメモリを割り当ててもらい、新しいStringクラスオブジェクトを作って文字列を表すのだ。

このように新しい文字列が作られれば、<span style="color:blue">"既存の文字列"はガベージコレクターによって除去されるべきデメリット</span>(いつ除去されるかわからない)がある。

また、このような文字列演算が多くなれば、演算一度するたびに<span style="color:blue">文字列オブジェクトを作るオーバーヘッドが発生</span>するので、性能は劣らざるを得ない。 (+演算に内部的にchar配列を使用する)

その代わり、Stringクラスのオブジェクトは<span style="color:blue">不変</span>なため、単純に読んでいく照会演算では他のクラスより早く読めるという長所がある。 (追加の長所に<span style="color:blue">不変</span>なため、<span style="color:blue">マルチスレッド環境で同期化を気にする必要がない。</span>)

### Stringクラスが適切な場合

結論的に、String クラスは文字列演算が少なく、頻繁に参照(照会)するときに使うとよい。 特にマルチスレッド環境で気にすることがなくていい。

### StringBuffer vs StringBuilder?

StringBufferとStringBuilderクラスはStringと違ってmutable(変更可能)である。

つまり、文字列演算をするとき、クラスは一度だけ作り(new)、メモリの値を変更させて文字列を変更する。

だから、いたずらに途中で文字列データを作らなくなるから、<span style="color:blue">文字列演算がよくある時に使うと性能が良い。</span>

しかもString BufferとString Builderクラスのメソッドが同じなので互換が可能である。

では、String BufferとString Builderの差は何だろう?

違いはString Bufferはマルチスレッド環境でsynchronizedキーワードが可能なので同期が可能である。 つまり、thread-safe である。 逆にString Builder はthread-safe していない。

String Builderは同期をサポートしないため、マルチスレッド環境では不向きである。

その代わり、StringBuilder が同期を考慮しないため、シングルスレッド環境でStringBuffer に比べて演算処理が早いメリットがある。

### String Buffer、String Builderが適切な場合

結論として、文字列演算が多いときに2クラスを使うが、マルチスレッド環境ではStringBufferを使うと良く、シングルスレッド環境やマルチスレッドであっても敢えて同期が必要ない場合にはStringBuilderを使うとよい。

### まとめ

<span style="color:blue">Stringクラスは不変</span>オブジェクトであるため、文字列演算が多いプログラミングが必要な時、続けてインスタンスを生成するので性能は落ちますが、照会の多い環境、マルチスレッド環境で性能的に有利です。

<span style="color:blue">StringBufferクラスとStringBuilderクラス</span>は、文字列演算が頻繁に発生するときに文字列が変更可能なオブジェクトであるため、性能的に有利です。

String BufferとString Builderの違いは、<span style="color:blue">同期サポートの有無</span>で同期を考慮しない環境でString Builderの方がより性能がよく、同期が必要なマルチスレッド環境ではString Bufferを使用するのが有利です。

[参照][string]

[string]: https://jeong-pro.tistory.com/85 "string"