---
layout: post
categories: "Linux"
title: "Vi Command"
author: "TY_K"
---

#### 명령 모드(command mode)

* x - 한글자 삭제 (5x: 문자 5개 삭제) 
* dw - 커서가 위치한 곳에서 부터 단어 삭제 (커서가 위치한 곳 부터 띄어쓰기 까지)

* dd - 커서가 위치한 곳의 한 줄 버퍼로 잘라내기 (잘라내기이지만)
* yy - 현재 줄을 버퍼로 복사 (5yy: 5줄 복사)
* p - 현재 커서가 있는 줄 바로 아래에 버퍼 내용 붙여넣기

* u - 방금 한 명령 취소 (ctrl + z 효과)
  
* i - 현재 커서 위치에 삽입 (입력모드로 넘어감)  
* a - 현재 커서 바로 다음위치에 삽입 (입력모드로 넘어감) 
* o(영문) - 현재 줄 다음 위치에 삽입 (입력모드로 넘어감)
* esc - 입력모드 취소

* 0(숫자) - 커서가 있는 줄의 맨 앞으로 감 (home)
* $ - 커서가 있는 줄의 맨 뒤로 감 (end)

* ( - 현재 문장의 처음
* ) - 현재 문장의 끝
* { - 현재 문단의 처음
* } - 현재 문단의 끝

* G 파일의 끝으로 이동

#### 마지막 행 모드(Last line mode)
: 으로 진입
* wq - 저장하고 종료
* q! - 강제 종료
* e - 마지막 저장 이후 모든 편집 취소

* 숫자 - 해당 라인으로 커서 이동
* $ - 파일의 맨 끝 줄로 이동 

* / - 현재 커서 위치에서 부터파일 앞쪽으로 문자열 탐색
* ? - 현재 커서 위치에서 부터 파일 뒤쪽으로 문자열 탐색 

* set nu - vi 라인 번호 출력
* set nonu - vi 라인 번호 출력 취소

[참조][link1]

[link1]: https://blockdmask.tistory.com/25 "link1"