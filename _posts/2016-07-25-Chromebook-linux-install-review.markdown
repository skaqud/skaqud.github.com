---
layout: post
title: 크롬북 Crouton을 이용한 Linux 설치 사용기
date: '2016-07-25 12:21:15 +0700'
tags:
  - life
  - review
  - hobby
published: true
---

Asus C201(정확한 모델명은 C201PA-DS02) 크롬북에 crouton을 통해 ubuntu(with xfce)를 설치하고 사용해본 7일 사용소감.

# 소개

구입 목적은 매일, 막, 꺼리낌 없이 가지고 다닐 노트북의 필요성으로, 맥북프로 13인치는 무게도 그렇고 분실위험도 있어서 아무래도 좀 부담스러웠음. 아마존을 통해 구매대행으로.. 는 귀찮아서, 그냥 국내 구매대행 사이트를 통해 20만원 초반대로 구입함.

## Spec

- Rockchip 1.8 GHz Processor
- 4 GB DDR3 RAM
- 16GB SSD Storage; No Optical Drive
- 11.6 inches 1366*768 pixels LED-lit Screen
- Chrome Operating System; Navy Blue Chassis

## 장점
가벼움, 오래가는 배터리(10시간이 뻥이 아닌듯), 준수한 사양(1.8GHz cpu,mem 4GB)

## 단점
CPU가 ARM계열이라 동작하지 않는 프로그램들이 꽤 있음, crouton의 제약으로 설치가 힘듬, 크롬북의 터치패드는 또다른 장점이 있으나, Mac과 그 방향이 반대임, 일부는 더 편한 제스처도 존재함. 크롬OS는 리눅스와 같이 사용시 약간의 불안정함이 있음.

전체적인 소감은 액정은 막눈이라 모르겠고, 만듦새도 가격대비 나쁘지 않은 편인 듯, 무게는 맥북프로에 비해 정말 가벼움
다만 아직은 크롬OS는 제약이 좀 많다.

# Crouton을 통한 Linux 설치

우선 poin2 블로그처럼, crouton으로 리눅스를 설치하는 방법은 웹에서 꽤 쉽게 찾을 수 있다.

참고 - [poin2블로그-crouton으로 리눅스 사용하기](http://blog.poin2.com/2015/09/크롬북에서-crouton-사용하기/)

위의 내용들 참고하여 개발자모드로 변경한 뒤 crouton 쉘에서 다음과 같이 입력하여 설치

다들 unity 보다는 xfce, lxde 등 가벼운 런처를 권장한다. 보통 크롬북이 2GB램 으로 나와서도 그런듯. lxde보다는 xfce가 ui나 기능 측면에서 더 좋은 편이다. unity로 설치시 오류가 나서 시도도 못해봤음.

다만, 설치시 우분투 설치 후 설정에서 삽질을 다소 하게 되는데, 아래를 참고해서 설치했다.

참고 - [도시바 크롬북2 우분투 설치 14.04](http://bookstorycabin.tistory.com/entry/도시바-크롬북2-chromebook-2-우분투-ubuntu-설치-1404)

요점은, 설치 후 확인되지 않은 경우 ubuntu-software-center만을 이용해서 설치하라고 함, update 서비스나, apt-get을 이용하려고 하면 에러가 발생하는 경우가 많다. 익숙해지기 전까지는 위에서 알려준 대로, software-center 설치 후 language support와 IBUS를 설치하면 일단 초기세팅은 무리 없이 끝나는 듯 하다.(신경써서 하지 않으면 계속 설치를 반복하게 됨.. 의외로 시간이 걸린다.)


# crouton 사용법

설치시 몇번에 걸쳐 재설치를 해야만 하는 경우 다음과 같은 명령어들을 사용

참고 - [How to Manage the Crouton Linux System on Your Chromebook](http://www.howtogeek.com/210047/how-to-manage-the-crouton-linux-system-on-your-chromebook/)

    #설치된 디렉토리 위치
    /mnt/stateful_partition/crouton/chroots/($osname)

    #설치 가능한 목록보기
    sudo sh ~/Downloads/crouton -r list

    #설치(desktop을 xfce기반으로)
    sudo sh ~/Downloads/crouton -r trusty -t xfce
    #설치(desktop없이 cli만)
    sudo sh ~/Downloads/crouton -r trusty -t extra-cli
    #설치(ubuntu 16.04, cli만) - 2017.05 재설치시 추가
    sudo sh ~/Downloads/crouton -r xenial -t core

    #잘못 설치했을 경우 삭제
    sudo delete-chroot name

    #설치된 OS시작
    sudo startxfce4
    #혹은(cli일때)
    sudo startcli

    #cli로 시작 후 환경에 접속
    sudo enter-chroot


# Linux 설정 및 사용

초기 설치

    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get install software-center

소프트웨어 센터 설치 후 sw center에서 language-support, ibus-hangul 설치함.

처음에는 브라우저 상에서 정상적으로 한글이 나오지 않음

language-support 를 실행시킨 뒤 Korean 언어를 설치해준다.

시스템의 로케일 설정

    # cat /etc/default/locale
    LANG="ko_KR.utf8"

로케일 설정후 시스템을 재기동하면 전체 메세지가 한글로 변경됨 이후 한글 입력에 문제가 생기면

    ibus-setup
    ibus-setup-hangul

둘을 차례로 재기동해 본다. --> 이래도 잘 안되는 경우가 있음.


이후 설치

- openjdk-7-jdk(apt-get) - 설치됨
- firefox(ubuntu-software-center) - 설치됨
- Eclipse(ubuntu-software-center) - 설치됨
- sublime text 는 시도중 - 설치 불가
- dropbox - 설치 불가
- atom - 설치 불가(xiwi로 별도 창으로 띄워서는 가능한 듯 싶음)


이클립스로 단순한 java 소스는 컴파일 가능했고, 무리 없이 사용 가능했다. sublime-text, atom 등 요즘 많이 쓰이는 개발도구들은 쉽게 설치해서 쓰기는 힘듬, 급한대로 vim으로 써야 할 것 같다.

덧. 그냥 콘솔만 띄워서 쓰고 있음, 브라우저는 어쩔 수 없이 크로미움을 설치

덧2. 한글사용이 문제가 많다. 됐다 안됐다 함.... 나처럼 github에 있는 파일만 수정하기 위해서라면, cli모드로 설치한 뒤 git으로 파일을 연동하고 크롬 앱으로 문서만 작성하는 것도 나쁘지 않을 것 같다.(Text 크롬 앱 이용)


## Linux 사용관련 소감

- linux 설치 후 chrome <-> linux는 생각만큼 원활하지는 않다. 단축키로 이동 가능하나(Ctrl+Alt+Shift+<- 와 ->)한쪽에 너무 많은 부하(?)를 줄 경우 지멋대로 재부팅을 하기도 함.(버그가 있는듯) 간혹 Linux에 넘어갔을 때 크롬북을 그냥 덮으면, 왠지 화면을 계속 띄워놓고 있는 듯한 느낌이 든다. 배터리 소모도 그렇고, 여는 순간 계속 화면이 켜있던 느낌이 듬.

- 생각보다 기존 PC와 키보드가 달라, 리눅스 사용시 적용이 좀 힘들다. 터미널에서 Insert키를 자주 쓰는데, 없다...ㅡ.,ㅡ;

- 크롬OS에서도 브라우징이 생각만큼 빠르지 않다. 가장 체감이 될 때는 웹페이지에서 뒤로 갈 때.

- galium os 등 크롬북 특화 OS 설치 불가능(CPU가 ARM계열의 Rockchip이라 다른 크롬북 기반의 OS를 설치할수도 없고 crouton일 때도 설치할 수 있는 desktop환경에 제약이 있다. 이런게 불편하면 맘편히 인텔기반 크롬북을 사는 게 좋다.)

- Playstore 업데이트 - 언제 생길지 아직 기약이 없음.

- C201의 경우 일부 데스크탑 환경이 정상적으로 설치되지 않음, CPU 문제인 듯, Unity, gnome 안되고, xfce, kde, lxde 는 됨. xfce가 가볍고 사용하기도 적당한 것 같음

- Crouton으로 실행시 no screen found 에러가 발생할 경우 다음과 같이 실행

     참고 - https://github.com/dnschneid/crouton/wiki/Asus-C201

     sudo enter-chroot
     sudo rm /dev/dri/card0
     sudo mv /dev/dri/card1 /dev/dri/card0


## 참고

- [crouton in github](https://github.com/dnschneid/crouton)
- [poin2블로그-crouton으로 리눅스 사용하기](http://blog.poin2.com/2015/09/크롬북에서-crouton-사용하기/)
- [도시바 크롬북2 우분투 설치 14.04](http://bookstorycabin.tistory.com/entry/도시바-크롬북2-chromebook-2-우분투-ubuntu-설치-1404)
- [How to Manage the Crouton Linux System on Your Chromebook](http://www.howtogeek.com/210047/how-to-manage-the-crouton-linux-system-on-your-chromebook/)
