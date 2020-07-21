---
layout: post
title:  "Linux 기본환경 구성하기"
date:   2016-01-03 11:20:15 +0700
tags:
- reuse
- 삽질
---

* TOC
{:toc}

---

vagrant 등을 사용하여 vm을 올릴 경우 매번 환경이 초기화되어, 번거로운 부분들이 있는데, 이런 부분들에 대해 정리해두고 매번 삽질하지 않도록 하기 위함임

다음과 같은 내용들을 포함합니다.(ubuntu 14,16,18,20 버전 대상, 이 문서는 지속적으로 보완중입니다.)

- repository 설정
- shell설정
- git 설정

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

### zsh 설치

    $ apt-get install zsh

Oh My Zsh 설치

    # 2020.06 업데이트 from https://github.com/ohmyzsh/ohmyzsh
    $ sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
    # powerline10k
    $ git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/themes/powerlevel10k
    
참고 - http://nolboo.kim/blog/2015/08/21/oh-my-zsh/

~~테마는 agnoster 등이 적당함. 설치시 git repository 표현에 몇몇 특수문자들이 필요하며, 폰트를 별도로 설치해 주는 게 좋다.~~

- [powerline10k](https://github.com/romkatv/powerlevel10k#oh-my-zsh) - (2020.06 업데이트)
~~- [agnoster.zsh theme](https://gist.github.com/agnoster/371287)~~
~~- [Powerline font](https://github.com/powerline/fonts)~~

powerelevel10k 를 받은 뒤 다음과 같이 zshrc 파일 안에 설정함(Set ZSH_THEME="powerlevel10k/powerlevel10k" in ~/.zshrc
)
바로 위에 가서 받으면 됨.

우분투 상에서 받은 otf 폰트는 /usr/share/fonts/opentype 하위에 넣으면 시스템에서 인식한다.(없을 경우 생성)

아래 테마,폰트가 linux에는 더 맞는 듯 하다.

- [gruvbox 테마](https://github.com/morhetz/gruvbox)
- [Fantasque Sans Mono font](https://github.com/belluzj/fantasque-sans)

### oh-my-zsh 플러그인

- [Oh my zsh with autosuggestions & syntax-highlighting](https://gist.github.com/dogrocker/1efb8fd9427779c827058f873b94df95)

### 기타 플러그인

oh-my-zsh를 미리 설치한 이후,

zsh-syntax-highlighting

    git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting

zsh-auto-suggestion
    
    git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions

위의 두개를 받은 이후 .zshrc 파일 안에

    plugins=(... zsh-autosuggestions zsh-syntax-highlighting ...)

와 같이 추가

zsh-suggestion 자동완성 단축키는 우측화살표/End

