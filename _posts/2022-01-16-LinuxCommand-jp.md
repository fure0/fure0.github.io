---
layout: post
categories: "Linux"
title: "Linux Command"
author: "TY_K"
---

### Linux Command

#### exit
* 닫기

#### man ls(명령어)
* 매뉴얼 보기

#### su
* 스위칭 유저

#### sudo
* root권한으로 실행

#### sudo su
* 루트 권한으로 변경

#### アカウント追加
* adduser - 대화형으로 추가
* useradd - 명령형으로 추가

#### sudo passwd [id]
* 패스워드 변경

#### 사용자 정보 변경
* chfn 대화식 변경
* chsh 대화식으로 사용자 쉘 변경
* usermod 비 대화식 변경

#### test라는 그룹에 해당 id를 넣는법
* ex) sudo usermod -G[test[] [id]

#### 해당 소유의 그룹 변경
* chgrp

#### 계정삭제
* deluser - 대화형으로 삭제
* userdel - 명령형으로 삭제
* 옵션 -r 홈디렉토리와 메일 저장소도 삭제
* 옵션 -f 포스

#### CD
* 디렉토리 이동 

> 상대경로(현재 내 위치에서의 경로를 쓸 때)

#### ./a
* 현재 디렉토리에 있는 a

#### ../a
* 상위 디렉토리에 있는 a

#### ~ 
* 자신의 홈디렉토리

> 절대경로(경로 전체를 쓸 때)

#### /home/yeon/a

#### ls
* 해당 디렉토리에 있는 파일들의 목록을 보여준다.
* 옵션 -a, -al 등 존재

#### a
* all 을 의미 숨김파일 표시됨(.으로 시작하는 파일은 숨김파일)

#### l
* 리스트 형태로 표현해줌

#### h
* 용량을 표기해주나 l과 형식으로 표현해야 보임

#### r
* 역순

#### R
* 디렉토리 내용까지 상세

#### s
* 기본정렬 이름순

#### *
* 해당 내용을 전부 보여줌(와이들 카드 라고 읽음)

ex) ls e* e로 시작하는 파일 전부

ex) e?o* e로시작하며 둘째는 아무것이 와도 되고 셋째는 o과 오는 파일 전부

#### 읽는법
-rw-r--r-- 1 root root 35 2014-03-07 09:54 minicom.log

drwxr-xr-x 4 root root 4096 2012-01-02 16:19 test

lrwxrwxrwx 1 root root 4 2016-02-22 08:26 test-ln -> test

1번째 값은 파일인지,디렉토리인지, 링크인지 확인 가능

d : 디렉토리, - : 일반파일, l : 링크

#### ACL access controll list
rwx(읽기,쓰기,실행)

2,3,4번째 값 : 소유자의 권한 

5,6,7번째 값 : 그룹의 권한

8,9,10번째 값 : 기타 사용자의 권한

#### chmod
* 파일에 대한 권한 설정

#### chmod 755 - R 
* 하위 폴더까지

#### pwd 
* 현재 내위치를 알려준다

#### clear
* 화면 지우기(화면 위로 밀기)

#### 파일/디렉토리 검색 명령어
* find / -name wonderit
* find 검색시작위치 이름으로검색 검색어

#### grep
* 필요한 정보 뽑아낼 때 사용

#### 명령어 결과에서 내가 필요한 부분만 찾을 때
* ps -ef | grep test
* ps -ef 의 결과에서 test가 포함된 것만 보여줘.

* grep test a.c
* a.c 파일에서 test가 있는 부분을 보여줘

#### PS
* 프로세스 리스트 보기

#### top
* 프로세스 점유율

#### cp
* 복사
* cp [복사할파일명] [복사될파일명]
* -r 하위 목록까지 복사

#### mv
* 파일 이동, 파일 이름변경
* mv a.c a.e 라고 하면 파일이 름이 a.c가 a.e 로 변경
* mv a.c [디렉토리 이름] 하면 해당 디렉토리로 이동

#### rm
* 파일 삭제
* -r 하위 디렉토리도 삭제
* -f 강제삭제

#### tar
* tar묶기
#### gz
* gz로 압축

#### df
* 디스크 용량 확인
* df-h 휴먼 사이즈 (Byte 단위로 표기)

#### du
* 파일의 사이즈를 표기
* du --max-depth=1 ./

#### head
* 파일 앞부분 10줄 보여주기

#### tail
* ファイルの後파일 뒷부분 10줄 보여주기ろの10行を表示

#### cat
* 파일 전체 내용 보기

#### mkdir
* 디렉토리 만들기
* -p 기술된 경로가 없으면 자동으로 만듬 ex) mkdir test1/test2/test3

#### rmdir
* 디렉토리 삭제하기

#### touch
* 빈 파일 만들기 ex) touch test

#### zip
* Zip압축 하기

#### unzip
* Zip파일 풀기

#### w 
* 해당 터미널에 어떤 사람들이 접속해 있는지 나옴

#### finger -l 
* 사용자에 대한 정보를 자세히 보여줌

#### tty
* 자신의 터미널 정보

#### ifconfig
* 자신의 ip를 확인

#### write
* 접속자들과 대화 가능 pts 번호로 /대화 중지는 ctrl + d

#### wall
* 전체 접속자에게 메시지를 보낸다

#### whoami
* 자기가 누구인지 알아보는 명령어

#### id
* 좀더 상세한 자신의 정보를 보여줌

#### uname -a
* 해당 커널 정보를 본다, 리눅스 버전등 정보가 나옴, 해킹할때 이걸 먼저 확인함

#### /etc/*release
* 설치된 OS 버전 확인

#### rpm -qa
* 패키지 정보 확인

#### cat /proc/cpuinfo
* cpu정보 확인

#### cat /etc/passwd

#### cat /etc/shadow
* 위의 passwd 파일 암호 노출을 막기 위해 이용