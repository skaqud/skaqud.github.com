---
layout: post
title:  "크롬북 Crouton을 이용한 Linux 설치 사용기"
date:   2016-07-25 12:21:15 +0700
categories: reuse
---

Asus C201(정확한 모델명은 C201PA-DS02) 크롬북에 crouton을 통해 ubuntu(with xfce)를 설치하고 사용해본 7일 사용소감.

#소개

구입 목적은 매일, 막, 꺼리낌 없이 가지고 다닐 노트북의 필요성으로, 맥북프로 13인치는 무게도 그렇고 분실위험도 있어서 아무래도 좀 부담스러웠음. 아마존을 통해 구매대행으로.. 는 귀찮아서, 그냥 국내 구매대행 사이트를 통해 20만원 초반대로 구입함.

### Spec

- Rockchip 1.8 GHz Processor
- 4 GB DDR3 RAM
- 16GB SSD Storage; No Optical Drive
- 11.6 inches 1366*768 pixels LED-lit Screen
- Chrome Operating System; Navy Blue Chassis

### 장점
가벼움, 오래가는 배터리(10시간이 뻥이 아닌듯), 준수한 사양(1.8GHz cpu,mem 4GB)

### 단점
CPU가 ARM계열이라 동작하지 않는 프로그램들이 꽤 있음, crouton의 제약으로 설치가 힘듬, 크롬북의 터치패드는 또다른 장점이 있으나, Mac과 그 방향이 반대임, 일부는 더 편한 제스처도 존재함. 크롬OS는 리눅스와 같이 사용시 약간의 불안정함이 있음.

전체적인 소감은 액정은 막눈이라 모르겠고, 만듦새도 가격대비 나쁘지 않은 편인 듯, 무게는 맥북프로에 비해 정말 가벼움
다만 아직은 크롬OS는 제약이 좀 많다.


# Crouton을 통한 Linux 설치

참고

위의 내용들 참고하여 개발자모드로 변경한 뒤 crouton 쉘에서 다음과 같이 입력하여 설치

다들 unity 보다는 xfce, lxde 등 가벼운 런처를 권장한다. 보통 크롬북이 2GB램 으로 나와서도 그런듯. lxde보다는 xfce가 ui나 기능 측면에서 더 좋은 편이다. unity로 설치시 오류가 나서 시도도 못해봤음.

다음 내용을 참고하여 설치

우선 poin2 블로그처럼, crouton으로 리눅스를 설치하는 방법은 웹에서 꽤 쉽게 찾을 수 있다.

- 참고 - (poin2블로그-crouton으로 리눅스 사용하기)[http://blog.poin2.com/2015/09/크롬북에서-crouton-사용하기/]

다만, 설치시 우분투 설치 후 설정에서 삽질을 다소 하게 되는데, 아래를 참고해서 설치했다.

- 참고 - (도시바 크롬북2 우분투 설치 14.04)[http://bookstorycabin.tistory.com/entry/도시바-크롬북2-chromebook-2-우분투-ubuntu-설치-1404]

요점은, 설치 후 확인되지 않은 경우 ubuntu-software-center만을 이용해서 설치하라고 함, update 서비스나, apt-get을 이용하려고 하면 에러가 발생하는 경우가 많다. 익숙해지기 전까지는 위에서 알려준 대로, software-center 설치 후 language support와 IBUS를 설치하면 일단 초기세팅은 무리 없이 끝나는 듯 하다.


# crouton 사용법

설치시 몇번에 걸쳐 재설치를 해야만 하는 경우 다음과 같은 명령어들을 사용

- 참고 - (How to Manage the Crouton Linux System on Your Chromebook)[http://www.howtogeek.com/210047/how-to-manage-the-crouton-linux-system-on-your-chromebook/]

    #설치된 디렉토리 위치
    /mnt/stateful_partition/crouton/chroots/($osname)

    #설치 가능한 목록보기
    sudo sh ~/Downloads/crouton -r list

    #설치
    sudo sh ~/Downloads/crouton -r trusty -t xfce

    #잘못 설치했을 경우 삭제
    sudo delete-chroot name

    #설치된 OS시작
    sudo startxfce4



# Linux 설정 및 사용

이후 설치한 것들은 다음과 같음

- openjdk-7-jdk(apt-get) - 설치됨
- firefox(ubuntu-software-center) - 설치됨
- Eclipse(ubuntu-software-center) - 설치됨
- sublime text 는 시도중 - 설치 불가
- dropbox - 설치 불가
- atom - 설치 불가(xiwi로 별도 창으로 띄워서는 가능한 듯 싶음)


이클립스로 단순한 java 소스는 컴파일 가능했고, 무리 없이 사용 가능했다. sublime-text, atom 등 요즘 많이 쓰이는 개발도구들은 쉽게 설치해서 쓰기는 힘듬, 급한대로 vim으로 써야 할 것 같다.
