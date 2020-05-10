---
layout: post
categories: "JS"
title: "vue js lazy load"
author: "TY_K"
---

### lazy load

ReactJS、Vue.jsのようなフロントエンドフレームワークの基本的な属性はユーザが初めて進入する際にプロジェクトと関連する全てのリソースを一気にダウンロードする。 

現在見ている画面と無関係なものまで全て。 このため、プロジェクトが少しだけ成熟してもリソースの量が急増し、リソースをダウンロードする時間が長くなる。 

Lazy loadはリソースをコンポーネント単位で分離させて、コンポーネント、あるいはルータ単位で必要なものだけをダウンロードできるようにする。

もちろん基本的に不要なリソースを減らしたり、CDN を積極的に活用するといった工夫が必要であるが、Lazy load もフロントエンドフレームワークプロジェクトにおいて必須の方法となっている。

### 方法

Vuecliを使用してビルドを運営しているなら、簡単に適用することができる。 ルータの設定に必要なコンポーネントを下記のようにインポートすると、自動的にビルド時にwebpackChunkNameが設定され、その基準でリソースが分離されてビルドされる。

```javascript
const Home = () => import(/* webpackChunkName: "home" */ './Home.vue');
const About = () => import(/* webpackChunkName: "about" */ './About.vue');
const Contact = () => import(/* webpackChunkName: "contact" */ './Contact.vue');
const routes = [
  { path: '/', name: 'home', component: Home },
  { path: '/about', name: 'about', component: About },
  { path: '/contact', name: 'contact', component: Contact }
];
```

[参照][lazy]

[lazy]: https://medium.com/@jeongwooahn/vue-js-lazy-load-%EC%A0%81%EC%9A%A9%ED%95%98%EA%B8%B0-b1925e83d3c6 "lazy"