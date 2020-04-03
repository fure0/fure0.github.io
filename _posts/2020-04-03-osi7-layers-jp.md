---
layout: post
title: "OSI 7 階層(OSI 7 Layers)"
author: "TY_K"
---

<style>
    .post img {
        margin : 0
    }
</style>

![OSI_7_Layers](https://user-images.githubusercontent.com/20508342/78373084-11934680-7605-11ea-998f-8e82e05dbc05.png){: width="80%" height="80%"}

# OSI 7 階層(OSI 7 Layers)とは?

## 7階層は何故区分するのか？
通信が起こる過程を段階別に知ることができ、特定の場所に異常が生じれば、その段階だけ修正できるためだ。


## 1) 物理(Physical)
> リピーター、ケーブル、ハブなど

ただデータ電気信号に変換して交換する機能を進める空間

つまり、データを転送する役割のみを行う。

## 2) データリンク(Data Link)
> ブリッジ、スイッチなど

物理階層に送受信される情報を管理して安全に伝わるようサポートする役割

Macアドレスで通信する。 フレームにMacアドレスを付与し、エラー検出、再送信、フロー制御を行う。

## 3) ネットワーク(Network)
> ルータ、IP

データを目的地まで最も安全かつ迅速に伝達する機能を担う。

ルータで移動する経路を選択し、IPアドレスを指定し、その経路によってパケットを伝達する。

ルーティング、フロー制御、エラー制御、セグメンテーション等を行う。

## 4) 転送(Transport)
> TCP, UDP

TCP とUDP プロトコルにより通信を活性化する。 ポートを開き、プログラムが転送できるようにしてくれる。

TCP:信頼性、連結志向的

UDP:非信頼性、非連結性、リアルタイム

## 5) セッション(Session)
> API, Socket

データが通信するための論理的接続を担当する。 TCP/IP セッションを作成·無くす責任を持っている。

## 6) 表現(Presentation)
> JPEG、MPEGなど

データ表現に対する独立性を提供し暗号化する役割を担う。

ファイルエンコード、コマンドを包装、圧縮、暗号化する。


## 7) 応用(Application)
> HTTP、FTP、DNSなど

最終目的地であり、応用プロセスと直接関係して一般的な応用サービスを行う。

ユーザインタフェース、電子メール、データベース管理などのサービスを提供する。


[参照][osi7]

[osi7]: https://github.com/gyoogle/tech-interview-for-developer/blob/master/Computer%20Science/Network/OSI%207%20%EA%B3%84%EC%B8%B5.md "osi7"