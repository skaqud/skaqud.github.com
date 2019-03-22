---
layout: post
title: linux on dex 소개
date: 2019-03-21 20:21:00
tags:
- Tech
- Linux
- Dex
---

# linux on dex 설치

갤럭시탭s4에 dex on linux 설치 및 사용기


## 초기설치

app은 apkmirror에서 다운로드

Linux image는 [여기서](https://webview.linuxondex.com/) 다운로드 후 압축해제(약10GB), SD카드상에 놓으면 VM(?, 혹은 Docker 인스턴스라 해야 할까?)이 카드 상에 생성됨(속도가 약간 딜레이가 있음, 짜증날정도->가급적이면 내장메모리 큰 모델을 사자), 메인메모리에 생성하면 VM메인메모리상에 생성. 

## Linux환경설정

실행한 뒤 접속하면 
sudo apt upgrade 명령을 실행시 다음과 같이 나타남

	grep: /proc/modules: Permission denied
	grep: /proc/modules: Permission denied
	grep: /proc/modules: Permission denied
	grep: /proc/modules: Permission denied
	grep: /proc/modules: Permission denied
	W: mdadm: /etc/mdadm/mdadm.conf defines no arrays.
	W: mkconf: MD subsystem is not loaded, thus I cannot scan for arrays.
	W: mdadm: failed to auto-generate temporary mdadm.conf file.
	Unsupported platform.
	run-parts: /etc/initramfs/post-update.d//flash-kernel exited with return code 1
	dpkg: error processing package initramfs-tools (--configure):
	 subprocess installed post-installation script returned error exit status 1
	Errors were encountered while processing:
	 initramfs-tools

어플리케이션 등이 설치는 되나, 해당 에러가 계속 보여짐


### 한글설정

참고 : https://gurenareut.tistory.com/6

    sudo apt install fonts-nanum fonts-nanum-extra fonts-nanum-coding ibus-hangul

ibus-setup-hangul 실행 후 Control + Shift + space 키를 눌러 한영 전환키 추가 

데몬 자동실행 설정을 쉘 시작시 추가(.zshrc)

    ibus-daemon -drx


## 쉘 환경설정

폰트 다운로드 - Fantasque Sans Mono 1.7.2 
	
    https://github.com/belluzj/fantasque-sans/releases/tag/v1.7.2

압축을 해제한 뒤 /usr/share/fonts/ 하위에 otf, ttf에 맡게 넣어 둠(zsh의 agnoster테마등에서 화면이 깨지지 않게 해주는 폰트)


쉘 관련 프로그램들 설치

    
oh-my-zsh 

    sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"

zsh-syntax-highlighting
(https://github.com/zsh-users/zsh-syntax-highlighting/blob/master/INSTALL.md)

    git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
    # 다음 내용을 .zshrc에 추가
    plugins=( [plugins...] zsh-syntax-highlighting)

zsh-autosuggestions
(https://github.com/zsh-users/zsh-autosuggestions/blob/master/INSTALL.md)

    git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
    # 다음 내용을 .zshrc에 추가
    plugins=(zsh-autosuggestions)

## 개발도구


(작성중)