---
layout: post
title: Ubuntu Gnome 설치 및 사용법
date: 2017-08-02 11:23:00
tags:
- Tech
- 삽질
---

Ubuntu Gnome 초기 설치 및 세팅방법들, 참고로 17이후 버전에서는 unity를 버리고 다시 gnome으로 돌아간다고 함

# 설치 및 환경구성

gnome tweak tool 설치 후

    apt-get install gnome-tweak-tool

아래 링크의 내용을 참고하여 테마 등의 설정을 변경

[Ubuntu Gnome 14.04/15.04 설치 후 세팅](http://programmingsummaries.tistory.com/360)

한글 설정은 요새는 거의 문제가 없지만, 문제가 있을 경우 위 링크의 내용을 참고하여 해결.

필수라고 하는 dock-plank 설치

  $ sudo add-apt-repository ppa:ricotz/docky
  $ sudo apt-get update && sudo apt-get install plank

처음 설정은 plank --preference 후 창에서 변경하고, 이후 실행할 때마다 자동으로 로딩되도록 하려면 gnome-tweak-tool > 시작 프로그램 메뉴에 plank를 등록해준다.

알려진 테마는 Paper, Arc, Moka 등이 많이 쓰임 - icon은 Paper, Theme는 Arc로..

Paper 테마 설치

    $ sudo add-apt-repository ppa:snwh/pulp
    $ sudo apt-get update
    $ sudo apt-get install paper-gtk-theme paper-icon-theme

Numix 테마 설치

    $ sudo add-apt-repository ppa:numix/ppa
    $ sudo apt-get install numix-gtk-theme numix-icon-the me-circle numix-icon-theme
    $ sudo apt-get update



# 팁, 단축키

    #다중 데스크탑 이동
    Ctrl + alt + 위,아래

    #터미널 실행
    Ctrl + alt + T(Terminator 설치시 해당 프로그램이 실행됨.)

    #Terminator
    Ctrl-Shift-E: 수직분할
    Ctrl-Shift-O: 수평분할
    Ctrl-Shift-P/N : 이전/다음 창 이동(분할된 상태에서)
    Ctrl + alt + 화살표: 창의 spilt상태에서 크기를 조절 가능

# 유틸, 프로그램

Atom - Sublime text의 경우 한글 입력에 문제가 있음. linux라 램이 좀 널널해서, Atom이 그렇게 느린 느낌이 들지 않는다.

Visual Studio Code - mac, windows, linux에 깔아봤는데 쓸만한 편집기, python같은 경우 툴 자체에서 컴파일도 할 수 있을 것 같다.

Pinta - 간단한 이미지 편집용 프로그램으로 사용, UI는 포토샵과 유사하다. 그 이상의 기능은 사용해보지 않았음.

Dropbox - 다음과 같이 설치

  apt-get install nautilus-dropbox
