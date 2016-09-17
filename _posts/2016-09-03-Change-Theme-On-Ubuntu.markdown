---
layout: post
title:  "Ubuntu(Unity,Xfce)테마 변경하기"
date:   2016-09-03 21:11:33
tags:
- 삽질
- Tech
- Linux
---

노트북과 크롬북에서 사용중인, Ubuntu 14, 16에서의 테마 변경 방법


# Ubuntu 16.04(LTS,Unity) 경우

우선 unity-tweak-tool 설치

    $ sudo apt-get install unity-tweak-tool

설치 가능한 몇가지 테마가 있는데, Numix, Paper가 괜찮은 듯.

Numix 테마 설치

    $ sudo add-apt-repository ppa:numix/ppa
    $ sudo apt-get update
    $ sudo apt-get install numix-*

Paper 설치

    $ sudo add-apt-repository ppa:snwh/pulp
    $ sudo apt-get update
    $ sudo apt-get install paper-icon-theme paper-gtk-theme

이후 아래와 같이 unity-tweak-tool을 실행하여 theme와 icon을 변경해주면 됨, 가끔 잘 먹지 않으나 기다리거나 재부팅하면 된다.

참고 - [Ubuntu 16.04(LTS) 설치 후 세팅](http://programmingsummaries.tistory.com/389)

참고2 - [깔끔한 우분투 테마-Paper](http://programmingsummaries.tistory.com/344)


# Ubuntu 14.04(crouton버전,xfce) 경우


참고 사이트를 참고(?)하여 다음과 같이 설치

아이콘은 다음과 같이 설치, 잘 적용됨.

    $ sudo add-apt-repository ppa:elementary-add-team/icons
    $ sudo apt-get update
    $ sudo apt-get install elementary-add-icon-theme

테마는 다음으로 설치하려고 하였으나, 적용을 해도 기본으로 설치되어 있는 것과 크게 달라지지 않음...
(테마 설치 디렉토리는 /usr/share/themes/)

테마 - [axiom](https://www.xfce-look.org/p/1016679/)

gtk-2,3, xfce도 xfce4와 xfcem이 있는데, 무슨 차이인지..

crouton이 문제인지.. 딱히 원인을 모르겠음.

참고 - [Xfce4 Look 변경하기](http://tacitus-textcube.blogspot.kr/2010/06/xfce4-look-%EB%B3%80%EA%B2%BD%ED%95%98%EA%B8%B0.html)
