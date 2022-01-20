---
layout: post
categories: "JS"
title: "Type Script"
author: "TY_K"
---

<style>
    .post img {
        margin : 0
    }
</style>


# TypeScript란?

TYPE이 있는 javascript를 작성시 보통의 javascript로 컴파일한다.
Any browser、Any host、Any Os、Open source
편리한기능, ms개발

이하TypeScript는ts로 명칭

# 왜ts를 사용해야 되나？
타입이 지정가능해지는 것으로 작성하면서 실수를 줄일 수 있다.
코드로 데이터의 예측이 가능하진다.

# 간단히 작성해 보자

# ts 설치

npm install -g typescript
<img width="569" alt="install" src="https://user-images.githubusercontent.com/20508342/77920750-d5e13f80-72d9-11ea-9a3f-89ff26f6570a.png"> 
3.3.3버전이 설치된 것을 확인


테스트 코드 greeter.ts 를 작성
<img width="367" alt="greeter" src="https://user-images.githubusercontent.com/20508342/77921001-26589d00-72da-11ea-89a8-bb61efc5d8d3.png">

tsc커멘드로 ts로 컴파일.
<img width="397" alt="comfile" src="https://user-images.githubusercontent.com/20508342/77921192-691a7500-72da-11ea-8cd9-839c62333df6.png">
greeter.js로 컴파일 된것을 확인.
여기까지 변경점없이 보통의 js랑 같음

여기서 부터 ts의 기능을 사용해 보면
<img width="367" alt="greeter" src="https://user-images.githubusercontent.com/20508342/77921344-9cf59a80-72da-11ea-89d2-eb2a17b0026d.png">

greeter function의 인수로 string 타입을 추가
<img width="797" alt="cngValue" src="https://user-images.githubusercontent.com/20508342/77921479-c9111b80-72da-11ea-9a19-70f5c4e72fa8.png">
user를 array로변경하면, [ts] ‘number[]’ 형식의 인수는 ‘string’형식의 매개변수로 할당할 수 없음
라는 에러메세지 확인

<img width="631" alt="comfileError" src="https://user-images.githubusercontent.com/20508342/77921563-e5ad5380-72da-11ea-9907-936c9bc1036a.png">
IDE에서도 에러가 확인 가능하지만, 컴파일 시점에도 에러가 출력되는것을 확인

<img width="320" alt="error1" src="https://user-images.githubusercontent.com/20508342/77921638-fcec4100-72da-11ea-9b0d-08fb4b5dba9c.png">
마찬가지로 인수가 없는 경우도 적절한 에러를 출력

interface의 경우
<img width="539" alt="interfaceTs" src="https://user-images.githubusercontent.com/20508342/77921751-23aa7780-72db-11ea-9483-ebe530c55ea5.png">

interface을 작성후 컴파일 할 때
<img width="538" alt="interfaceJs" src="https://user-images.githubusercontent.com/20508342/77921806-30c76680-72db-11ea-97dc-329d72a461b6.png">
적절히 컴파일 된것을 확인

class의 경우도
<img width="809" alt="classTs" src="https://user-images.githubusercontent.com/20508342/77921907-4d639e80-72db-11ea-9379-8c97097afeef.png">

적절히 컴파일 된것을 확인
<img width="618" alt="classJs" src="https://user-images.githubusercontent.com/20508342/77921945-5b192400-72db-11ea-8f7d-5591d95ab52a.png">

마지막으로 html에 ts에서 js로 컴파일된 파일이 잘 작동하는지 확인
<img width="434" alt="greeterHtml" src="https://user-images.githubusercontent.com/20508342/77922021-771cc580-72db-11ea-90e1-61625488d0a0.png">

ts에 작성한 js가 문제 없이 동작하는 것을 확인
<img width="470" alt="greeterHtml2" src="https://user-images.githubusercontent.com/20508342/77922075-86037800-72db-11ea-9f5e-bc562bf98c3b.png">
