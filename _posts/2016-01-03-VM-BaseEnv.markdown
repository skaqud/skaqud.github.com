---
layout: post
title:  "VM 기본환경 구성하기"
date:   2016-01-03 11:20:15 +0700
tags:
- reuse
- 삽질
---

* TOC
{:toc}

---

vagrant 를 사용하여 vm을 올릴 경우 매번 환경이 초기화되어, 번거로운 부분들이 있는데, 이런 부분들에 대해 정리해두고 매번 삽질하지 않도록 하기 위함임

다음과 같은 내용들을 포함합니다.(ubuntu 14,16 버전 대상, 이 문서는 지속적으로 보완중입니다.)

- repository 설정
- shell설정
- git 설정
- vim 설정
- 그밖에?


# 환경설정

## Repository 설정

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

## 기본 어플리케이션 설치

git, wget, telnet, curl, tmux(multi screen terminal) 등 설치

    $ apt-get install git wget telnet curl tmux

zsh 설치(fish는 적응하기가 좀 힘듬)

    $ apt-get install zsh

기본 쉘 변경

    $ chsh -s /usr/bin/zsh

ubuntu, centos 에서 git은 최신 바이너리가 아닌 경우가 많음(항상 공식 repository에 버전업이 늦다.)
별도로 설치해야 할 경우 컴파일이 가장 빠르지만, 다음과 같이 수동으로 ppa를 추가한 뒤 설치한다.

    $ sudo add-apt-repository ppa:git-core/ppa
    $ sudo apt-get update
    $ sudo apt-get install git


# Shell 환경설정

zsh 설치

    $ apt-get install zsh

Oh My Zsh 설치

    $ curl -L https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh | sh

참고 - http://nolboo.kim/blog/2015/08/21/oh-my-zsh/

테마는 agnoster 등이 적당함. 설치시 git repository 표현에 몇몇 특수문자들이 필요하며, 폰트를 별도로 설치해 주는 게 좋다.

- [agnoster.zsh theme](https://gist.github.com/agnoster/371287)
- [Powerline font](https://github.com/powerline/fonts)

바로 위에 가서 받으면 됨.

우분투 상에서 받은 otf 폰트는 /usr/share/fonts/opentype 하위에 넣으면 시스템에서 인식한다.(없을 경우 생성)

아래 테마,폰트가 linux에는 더 맞는 듯 하다.

- [gruvbox 테마](https://github.com/morhetz/gruvbox)
- [Fantasque Sans Mono font](https://github.com/belluzj/fantasque-sans)

# vim 설치 및 설정

## 환경설정 ##

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
    #keymap setting
    nmap <F7> :NERDTree<CR>

## Vundle, NERDTree 설치 ##

참고 - [vim 플러그인 매니저, Vundle + NERDTree 플러그인 설치하기](https://dobest.io/install-vundle-and-nerdtree/)

참고2 - [github-VundleVIM](https://github.com/VundleVim/Vundle.vim)

아래와 같이 vundle 설치

    $ mkdir ~/.vim/bundle
    $ git clone https://github.com/gmarik/Vundle.vim.git ~/.vim/bundle/Vundle.vim
    #.vimrc 파일 생성(참고2 링크에 있음)
    #vim command mode에서 다음과 같이 입력
    :PluginInstall

vundle을 이용해 NERDTree 설치

    :PluginSearch NERD
    # .vimrc에 아래 내용 추가
    $ echo "Plugin 'The-NERD-Tree'" >> ~/.vimrc
    # 설치
    :PluginInstall

참고3 - [vim 사용자를 위한 플러그인 매니저 vundle 을 소개 합니다.](https://kldp.org/node/125263)

-> [vim-plug](https://github.com/junegunn/vim-plug)가 속도가 더 빠르고 편하다고 함


참고4 - [vimawesome](http://vimawesome.com)
