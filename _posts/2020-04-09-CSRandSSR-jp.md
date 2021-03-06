---
layout: post
categories: "web"
title: "CSR and SSR"
author: "TY_K"
---

### CSR & SSR

> CSR : Client Side Rendering

> SSR : Server Side Rendering

CSR にはモバイル時代に入り、SPA が登場した。

### SPA(Single Page Applictaion)
> 最初にページ全体をロードした後、データだけを変更して使用できるアプリケーション

SPA は基本的にページロードがなく、すべてのページが単にHtml5 History によってレンダリングされる

従来のSSR方式には性能問題があった。

最近、ウェブで提供される情報があまりにも多い。 要請するたびに読み込みが起こり、ページをロードするたびにサーバからリソースを渡されて解析し、画面にレンダリングする方式であるSSRは、データが多いほど性能問題が発生した。

これは、インタラクションの多い環境で非効率である。 レンダリングをサーバ側で進めると、その分サーバ資源が多用されるため不要なトラフィックが発生する。

CSR方式は、ユーザーの行動によって必要な部分だけ読み直してくる。 したがって、サーバ側からレンダリングして、全ページを読み込むより速いインタラクションが期待できる。 サーバはJSON ファイルのみを送信し、HTML を描く役割はJavaScriptを通じてクライアント側で行う方式である。

ビューレンダリングをユーザーのブラウザが担当し、先にウェブアプリをブラウザにロードした後、必要なデータだけを受け取って表示するCSRは、トラフィックを減少させ、ユーザーにより良い経験が提供できる。

### CSR長所と短所

### 長所

- トラフィック減少 
> 必要なデータだけを受け取る
- ユーザー経験
> リフレッシュなし。 ユーザーがネイティブアプリのような経験ができる
- 初期ローディング性能改善
> 最小限のデータだけを初めに画面に表示する

### 短所

- 検索エンジンのクローラーがデータ収集出来ない
> Google検索エンジンはJavaScriptエンジンが内蔵されているが、それ以外の検索エンジンはクローリングに困難があり、SSRを別途に実装しなければならない煩わしい存在。

### SSR長所と短所

### 長所
- 検索エンジンの最適化
> 全データが最初から画面に表示されるため、検索にかかる

### 短所
- 性能悪化の可能性
> 毎回資源をロードする