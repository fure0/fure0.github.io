---
layout: post
categories: "network"
title: "TCP-HandShake"
author: "TY_K"
---

<style>
    .post img {
        margin : 0
    }
</style>

# [TCP] 3 way handshake & 4 way handshake

> 連結成立、解除する過程

## 3 way handshake - 連結成立

> TCP は正確な転送を保障しなければならない。 したがって、通信する前に、論理的な接続を成立するために 3 way handshake 過程を進める。

![3wayHandShake](https://user-images.githubusercontent.com/20508342/78797014-18013400-79f2-11ea-9921-b626f346e8cb.png)

1. クライアントがサーバにSYNパケットを送信(sequence: x)

2. サーバがSYN(x)を受け取り、クライアントに受け取った信号であるACKとSYNパケットを送る(sequence:y、ACK:x_1)

3. クライアントは、サーバの応答ACK(x+1)とSYN(y)パケットを受け取り、ACK(y+1)をサーバに送信

このように3回の通信が完了すると、接続が成立する

## 4 way handshake - 接続解除

> 連結成立後、すべての通信が終わったら解除する。

![4wayHandShake](https://user-images.githubusercontent.com/20508342/78797054-26e7e680-79f2-11ea-82a7-355f7ec9e9be.png)

1. クライアントはサーバに接続終了のFIN フラグを送る。

2. サーバはFIN を受けて、確認のACKをクライアントに送る。 この時すべてのデータを送るためにTIME OUT状態になる

3. データを全て送信した場合、接続終了のFINフラグをクライアントに送る

4. クライアントはFIN を受けて、確認のACK をサーバに送る。 まだサーバから受け取っていないデータがあるかもしれないので、TIME_WAITを通じて待つ。

* サーバはACKを受け取ってからソケットを閉じる(Closed)

* TIME_WAIT 時間が終わればクライアントも閉じる (Closed)

このように4回の通信が完了すると、接続が解除される。

[参照][TCP-HandShake]

[TCP-HandShake]: https://github.com/gyoogle/tech-interview-for-developer/blob/master/Computer%20Science/Network/TCP%203%20way%20handshake%20%26%204%20way%20handshake.md "TCP-HandShake"