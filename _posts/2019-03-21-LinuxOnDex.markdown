---
layout: post
title: linux on dex 소개
date: 2019-03-21 20:21:00
tags:
- Tech
- Linux
- Dex
---

갤럭시탭s4에 dex on linux 설치 및 사용기

의식과 삽질의 흐름에 따라, 마음대로 정리합니다.


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


### 쉘 관련 프로그램들 설치

    
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

- eclipse, IntelliJ 사용 가능하나, 가용램이 3GB정도임, 개발도구 자체 실행은 되나 빌드 등의 작업 수행시 브라우저+개발툴+빌드툴+터미널(Java+Maven/Gradle등) 같이 수행하면 Linux 인스턴스 자체가 메모리 이슈로 이상 종료됨
  - 내가 산 모델이 64GB 모델이라, Swap을 더 확보하면 나아지는지 확인해보지는 않았음


vsCode 정도는 사용 가능할 것 같음(근데, 한글 입력이 안된다...ㅠ.ㅜ), Java이외의 언어, 무겁지 않은 경우 사용하기 좋을 것 같음(python, golang)

문서 작업용으로는 좋으나, Dropbox 를 쓰려고 하면 설치를 위해 컴파일 필요.
  - 외부 Shared Storage로 설정된 공간에 디렉토리를 만들어 안드로이드 환경에서 Dropsync등으로 동기화를 걸어놓고 사용하면 됨.


## 기타

- 블루투스 키보드 연결시 linux환경에서는 데스크탑과 동일한 환경 제공. 블투 마우스 혹은 무선 마우스를 USB허브 등으로 사용 가능함
    - 단, 화면에서 마우스 클릭시와 손으로 터치시 응답이 다름
    - 리눅스 화면상에서는 단축키등이 완전 이전 리눅스환경과 유사함, 덱스 홈 환경으로 나가면 또 좀 헷갈림.
    
- (메모리때문에) IDE 를 못쓰는데 왜 GUI를 쓰지? 라는 생각을 잠시 했음. 그러나 터미널과 브라우징이 컴퓨팅 환경과 동일하여 쓸만하다.

- 한글 입력은 여전히 되다, 안되다... 혼란스럽다(단축키 문제였음. Crtl+Shift+Space가 안된다면 다른 걸로 설정해보길..)

- 그러나, 가지고 다니면서 Linux를 사용하기에는 좀 불편하다.
   - RAM부족, Spec부족. Dex,LoD 안팎으로 오가다 보면 Linux환경이 강제 종료되어 있음
   - 터미널만 사용시에도 미묘한 느린 현상
   - 굳이 이렇게까지 태블릿을 이용해서 Linux를 써야 하나? -> 노트북을 들고다니지?

### 대안1 - Termux

안드로이드 기반의 Linux 가상환경. 많이 쓰이는 도구들을 포팅해 놓았음. 개발툴로도 사용할 수 있을 것 같음. git 등 개발도구들을 사용하며, 외부 안드로이드 앱을 활용할 수 있을 듯 함.

- 변변한 텍스트 에디터가 없음, 내 외장 저장공간을 왔다갔다 하기도 힘듦

- 붙여넣기는 화면을 길게 누르면 copy, paste 가능, 마우스나, 키보드로는 불가

- pkg install zsh 로 zsh 쉘 설치 후 chsh zsh 로 지정

- oh-my-zsh는 linux용을 변경한 버전이 있음 - https://github.com/Cabbagec/termux-ohmyzsh

