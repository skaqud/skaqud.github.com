---
layout: post
title:  "우분투16.04(LTS)에서 시스템 프로그램 오류를 발견했습니다. 에러시"
date:   2016-08-13 23:59:00
tags:
- life
- Tech
- Linux
---

우분투 16.04(LTS)를 노트북으로 사용중, 로그인 후 계속 발생하는 "시스템 프로그램 오류를 발견했습니다." (System program problem detected)에러 메세지를 해결하는 방법.

참고

[How to fix “System program problem detected” error on Ubuntu](http://www.binarytides.com/ubuntu-fix-system-program-problem-error/)

다음과 같이 crash report files들을 삭제하고 재시도

    % ls -ail                                                 16-08-15 - 22:32:10
    합계 560
    5767172 drwxrwsrwt  2 root   whoopsie   4096  8월 15 22:33 .
    5767169 drwxr-xr-x 14 root   root       4096  4월 21 07:19 ..
    5767564 -rw-r-----  1 colord whoopsie 563662  8월 11 21:55 _usr_lib_colord_colord-sane.113.crash

    % sudo rm /var/crash/*
    % sudo reboot

리부트 후 이상없음을 확인

차후로도 다른 이유로 같은 메세지가 뜨는 게 보기 싫다면 다음과 같이 apport를 끄면 된다고 함.

    #다음 파일을 연 뒤
    % sudo vi /etc/default/apport
    #다음과 같이 수정
    #enabled=1
    enabled=1
