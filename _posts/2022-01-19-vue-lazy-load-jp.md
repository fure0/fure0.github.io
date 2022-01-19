---
layout: post
categories: "JS"
title: "vue js lazy load"
author: "TY_K"
---

### lazy load

ReactJS, Vue.js 와 같은 프론트엔드 프레임워크의 기본적인 속성은 사용자가 처음 진입할때 프로젝트와 관련된 모든 리소스를 한번에 다운받는다. 

현재 보고 있는 화면과 무관한 것들까지 모두. 이때문에 프로젝트가 조금만 성숙해도 리소스의 양이 급증해서 리소스를 다운받는 시간이 늘어나게된다. 

Lazy load 는 리소스를 컴포넌트단위로 분리시켜주고, 컴포넌트 혹은 라우터 단위로 필요한 것들만 다운받을 수 있도록 한다.

물론 기본적으로 불필요한 리소스를 줄이거나, CDN을 적극 활용하는 다양한 방법이 필요하지만, Lazy load도 프론트엔드 프레임워크 프로젝트에서 필수적인 방법으로 자리잡았다.

### 방법

Vue cli 를 사용해 빌드를 운영하고 있다면 간단히 적용할 수 있다. 라우터 설정에 필요한 컴포넌트를 아래와 같이 import 하면 자동으로 빌드시 webpackChunkName 이 설정되며 그 기준으로 리소스가 분리되어 빌드된다.

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