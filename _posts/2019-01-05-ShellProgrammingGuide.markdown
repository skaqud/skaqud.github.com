---
layout: post
title: Shell Programming 가이드
date: 2019-01-05 13:21:00
tags:
- Tech
- Linux
- 
---

# Shell Programming 가이드

- 쉘 프로그래밍 작성시 주의해야 할 부분, 어떻게 작성해야 하는지
- Linux에 익숙하지 않은 개발자가 Devops환경에 적당히 적응하기 위한 쉘 사용법

p.s 순서는 작성자 마음대로, 작성자가 아는 내용만 적혀 있습니다. 더 자세한 내용은 검색하면 많습니다^^

---

# 기본적인 쉘 사용법

## 쉘이란?

- 사용자와 OS간의 인터페이스 역할
- 다양한 쉘의 종류 - bash, zsh, sh, csh, ksh, fish...
- bash가 기준, 현재는 zsh가 대세.
- GUI보다 CLI가 편하다?
- 자신에게 맞는 도구가 최선 
        - 쉘 스크립트는 개발용 언어가 아니다. 디버깅, 테스트 등이 매우(!) 어렵다. -> 참고의 고급Bash스크립팅 가이드 서문 참고
        - 대안을 선택 가능하다면 perl, python, golang등 고레벨 언어를 사용하자.

## 로그인 쉘 설정 및 적용

- .bashrc, .bash_profile 
    - bash에서 쉘 설정을 관리하는 파일
    - Login Shell, Non Login Shell
    - 현재 쉘 프로세스와 새로운 쉘 프로세스

- 수정방법
    - export - 환경변수 선언 명령, 명령과 Value 사이에는 공백이 없어야 한다.
    
    export http_proxy=xxx.xxx.xxx.xxx
    export PATH="$PATH:/Users/ska/anaconda3/bin:"
    export CLASSPATH="$CLASSPATH:/usr/lib/:/home/test/userlib/"
    - alias - 실수를 방지, 긴 명령을 짧게
    
    alias ll='ls -al', alias rm='rm -i'
    alias vhome="cd /Users/ska/vagrant"

    - . , source - 수정한 설정을 현재 쉘에 적용

## OS명령

- 확인명령
    - os version 확인 - cat /etc/*release* 
    - 커널 확인 - uname -a

- yum(centos)
    - yum search, yum clean all, yum install, yum repolist
    - rpm - rpm -qa | grep openjdk(설치도 rpm조회)

- apt(ubuntu)
    - apt(-get) install, apt search, apt clean all, apt update
    - dpkg - dpkg --get-selections | grep openjdk(설치된 패키지 조회)
- 기타
    - pwd, cd -, cd ~, sudo

---

# 쉘 스크립트 작성시 주의해야 할 부분

## 구조와 작성방법

- 쉘 프로그램의 구조(bash)
    - 특정한 제약은 없음
    - 시작 - #! /bin/sh - 주석으로 사용하는 쉘을 명시
- 주석, 연산자, 연결문
    - 주석 - #, 시작시 #!는 주석이 아님
    - 연산자 - 기본적인 연산자는 동일
    - 연결문
        - 한 라인이 길 경우, 혹은 가독성을 위해 여러 라인으로 작성 - \으로 연결(Windows에서는 ^)
        - &&  - 앞의 명령이 성공했을 경우 뒷 명령을 수행
        - & - 백그라운드로 실행, 뒤에도 명령이 있을 경우 앞의 문장을 백으로 실행하고 뒷문장을 실행
    - 조건문, 반복문의 사용 - 사용법이 일반 언어들과 상이
        - 조건문 if-fi, 조건은 [] 괄호 안에 쓴다
        - 비교연산자 - -lt,-gt,-le,-ge
        - 반복문 - for, while등
    - quotation - single, double, backquote
        - single - 따옴표 안의 값을 그대로 출력
        - double - 가장 일반적으로 사용, 쉘이 안의 값들을 치환하여 출력함
        - backquote - 명령어 치환,명령어를 안에 둘 경우 안에 든 명령의 결과가 표시됨

## 권한 문제 

- 권한 문제
    - 실행권한 chmod u+x file.txt 
    - rwx(읽기쓰기실행), 777 - 로 해당 파일에 접근 권한을 부여
- 소유자 설정
    - chown -R root:root ./directory
        - R - recursive
        - root:root - owner:group

## 문법 차이
- CMD(dos,powershell),다른 shell과의 문법 차이로 인한 혼동
    - export/set,  grep/findstr 등 기본 명령의 차이
    - 주석,연결문,연산자 등 기본 구조의 차이
    - os, shell별, 환경별 netstat 등 도구의 유무

# 다소 복잡한 쉘 프로그래밍

## ssh를 이용한 접속

    - 스크립트 안에서 ssh를 이용하여 접속하기 
    - scp - 파일 복사하기
    - sshpass - 자동 로그인 설정도구, 복잡해질 경우 fabric이나 ansible, saltstack등을 사용

## 비동기 작업 수행

- 작업을 걸고 집에 가고 싶을 때  - nohup, &
    - nohup(no hangup) - 현재 쉘 프로세스가 끝나도 작업은 유지됨
    - & - 프로세스를 백그라운드로 실행

## 연속된 일련의 실행
- 사용자에 입력을 받아야 하는 작업의 실행 - expect - 더 복잡한 작업의 경우 perl, python등 사용
    - Bash스크립트에 expect 넣기

## 스케쥴 걸기 
- crontab 명령을 이용해 작성된 shell스크립트 혹은 명령을 주기적으로 수행할 수 있음. 실행시 사용자, 권한 등 확인 필요

# 디버깅을 위한 방법

## 콘솔 출력
- 가장 단순하고 확실한 방법 - echo
- tee명령 - 화면과 파일 출력을 동시에

## 리다이렉션
- 리다이렉션 이용 - nohup 등으로 백그라운드로 실행되는 경우 결과가 화면에 보이지 않아 불편할 경우 사용
    - command >> filename.txt(append), command > filename.txt(create)
- 표준 입출력 이용 - 표준 입력(0), 표준 출력(1), 표준 에러(2) 처리방법. 
    - /dev/null로 출력을 보내어 화면에서 보이지 않게 할 수 있음

# 유용한 도구들

## 편집기-VI(emacs. pico..)
- 다양한 편집기 환경 - vi, vim, emacs, pico...
- 쉘 환경에서 vi를 에디터로 사용하기 
        - set -o vi - 커맨드라인 환경에서 vi의 단축키들을 사용 가능

## 커맨드라인 유틸리티들
- grep - 내용을 검색하는 명령, 여러개의 파일에서 빠르게 결과를 찾고자 할때
    
        #로그파일에서 error를 검색
        grep error *20190221.log
        #전 명령의 결과에서 특정 내용을 검색
        netstat -an | grep 3306 | wc -l

- find - 파일 찾기 명령, 파일 속성에 대해 검색, 별도 명령 실행 가능
    
        #수정한지 20일 이상된 파일만 삭제
        $ find . -mtime +20 -type f -ls -exec rm {} \;
        #확장자가 .htm* .gif, .js, .css 인 것만 퍼미션을 644(rw-r--r--)로    
        $ find . -name "*.htm*" -o -name "*.gif" -o -name "*.js" -o -name "*.css" -exec chmod 644 {} \;

- awk - 패턴을 가지고 넘어오는 결과를 검색
    
        #특정크기 이상의 로그파일 확인
        $ du -k | awk '{if (length($1) > 5) print}'
        $ du -k | awk '$1 > 100000'

- xargs - 이전 명령의 결과를 인자로 받음
    
        #특정 user의 process를 무조건 kill하는 command
        $ ps -ef | grep user | grep -v grep | awk '{print($1)}' |xargs kill -9

- 그밖에 - head, tail, sed...

## 네트워크 명령

- netstat
    - netstat 명령의 이용, 포트의 사용현황, 프로세스ID조회, -tunlp 옵션
    - centos 7.x 이후로 별도로 net-tools를 설치해야 사용 가능(ipconfig등)
- telnet, ssh, scp, ping
    - 방화벽 접속 확인, 파일 원격 복사 등

## 그밖에 

- curl - http요청을 보낼 수 있는 명령, RESTAPI 테스트도 가능
- wget - 파일 다운로드 명령
- tmux - 화면 분할, 다중화면 접속 도구
- htop - 진보된 top명령, top보다 나은 도구


# 참고
- [고급Bash 스크립팅 가이드(kldp)](https://wiki.kldp.org/HOWTO/html/Adv-Bash-Scr-HOWTO/)
- [bash입문자를 위한 핵심요약정리(개발자스럽다)](https://blog.gaerae.com/2015/01/bash-hello-world.html)
- [쉘을 이용한 프로그래밍](https://12bme.tistory.com/243?category=682900)
- [Shellcheck 사용](http://note.arzhna.net/2017/05/15/shellcheck%EC%9D%84-%EC%8D%A8%EB%9D%BC-%EB%91%90%EB%B2%88-%EC%8D%A8%EB%9D%BC/) - 정적 분석도구
- ssh에 대한 설명 -  생활코딩의 강좌[(SSH Key - 비밀번호 없이 로그인)](https://opentutorials.org/module/432/3742) 참고

