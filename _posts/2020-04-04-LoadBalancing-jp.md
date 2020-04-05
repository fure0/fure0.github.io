---
layout: post
categories: "network"
title: "LoadBalancing"
author: "TY_K"
---

<style>
    .post img {
        margin : 0
    }
</style>

![LoadBalancing](https://user-images.githubusercontent.com/20508342/78453183-073b8000-76cb-11ea-902b-9fa82e8b06b7.png)


# ロードバランシング(Load Balancing)

> 2つ以上のCPUorストレージなどのコンピュータ資源に作業を分けること

近頃では急速にウェブサイトにアクセスする人が増えた.

したがって、これらに対してすべてのトラフィックに対応するには、1台のサーバーでは足りない。 

対応策として、ハードウェアの性能を上げたり(Scale-up)、複数のサーバに分かれて働けるようにすること(Scale-out)がある。 

ハードウェア向上のコストがより高いこともあって、複数のサーバーで無中断サービスを提供する環境構成が容易であるため、スケールアウトが効率が良い。 その際、複数のサーバに均等にトラフィックを分散させるのがロードバランシングである。

ロードバランシングは分散式のウェブサービスで、複数のサーバに負荷(Load)を配る役割を持つ。 

Load Balancer をクライアントとサーバの間に置き、負荷が起きないように複数のサーバに分散させる方式である。 サービスを運営するサイトの規模によってウェブサーバーを追加増設し、ロードバランサーで管理すれば、ウェブサーバーの負荷を解決できる。

## ロードバランサーがサーバを選択する方式

* ラウンドロビン(Round Robin):CPUスケジューリングのラウンドロビン方式を活用

* Least Connections:接続数が最も少ないサーバーを選択(トラフィックによってセッションが長くなる場合を推奨)

* Source:ユーザーIPをハッシュして分配(特定のユーザーが常に同じサーバーにつながることを保障)

## ロードバランサー障害対策

> サーバを分配するロードバランサに問題が発生する可能性があるため、ロードバランサを二重化して備える。


[参照][LoadBalancing]

[LoadBalancing]: https://github.com/gyoogle/tech-interview-for-developer/blob/master/Computer%20Science/Network/%EB%A1%9C%EB%93%9C%20%EB%B0%B8%EB%9F%B0%EC%8B%B1(Load%20Balancing).md "LoadBalancing"