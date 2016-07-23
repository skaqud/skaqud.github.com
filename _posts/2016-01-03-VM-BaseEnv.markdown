---
layout: post
title:  "VM 기본환경 구성하기"
date:   2016-01-03 11:20:15 +0700
categories: reuse
---

# vm 기본환경 구성하기 #

vagrant 를 사용하여 vm을 올릴 경우 매번 환경이 초기화되어, 번거로운 부분들이 있는데, 이런 부분들에 대해 정리해두고 매번 삽질하지 않도록 하기 위함임

다음과 같은 내용들을 포함합니다.(ubuntu 14,16 버전 대상)

- repository 설정
- shell설정
- git 설정
- vim 설정
- 그밖에?


## apt-get(패키지관리자) 속도 개선 ##

repository를 daumkakao쪽 서버로 변경

    cd /etc/apt/
    cp sources.list sources.list.bak
    vi sources.list
    #vi 상에서 다음과 같이 입력하여 일괄 변경
    :%s/archive.ubuntu.com/ftp.daumkakao.com/g
    :%s/security.ubuntu.com/ftp.daumkakao.com/g
    # 변경정보 업데이트
    apt-get update

http://ftp.neowiz.com/ 로 변경해도 됨.

### 기본 어플리케이션 설치 ###

git, wget, telnet, curl, tmux(multi screen terminal) 등 설치

    apt-get install git wget telnet curl tmux

zsh 설치(fish는 적응하기가 좀 힘듬)

    apt-get install zsh

기본 쉘 변경

    chsh -s /usr/bin/zsh

ubuntu, centos에서 git은 최신 바이너리가 아닌 경우가 많음(항상 공식 repository에 버전업이 늦다.)
별도로 설치해야 할 경우 컴파일이 가장 빠르지만,


### Oh my zsh 설치 ###

curl -L https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh | sh

참고 - http://nolboo.kim/blog/2015/08/21/oh-my-zsh/


### vim 환경설정 ###

로컬의 .vimrc 안에 다음과 같은 내용을 넣음.

set hlsearch
set ts=4
set sts=4
set sw=1
set autowrite
set autoread
set bs=eol,start,indent
set history=256
set laststatus=2
set paste
set shiftwidth=4
set showmatch
set smartcase
set smarttab
set smartindent
set softtabstop=4
set tabstop=4
set ruler
