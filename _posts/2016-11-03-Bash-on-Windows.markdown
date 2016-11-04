---
layout: post
title: Bash on Windows
date: 2016-11-03 22:51:00
tags:
  - Life
  - 삽질
---

윈도우 10에서 비로소 사용 가능해진 bash 사용기 및 소감(작성중)

# 설치하기

이전까지 Windows에 bash를 설치하는 방법은 조금 복잡했으나, 윈도우 1주년 업데이트 후 좀 간단해진 것 같음, 우선 최신 업데이트까지 설치한 뒤,  설정 > 업데이트 및 복구 창에서 개발자용 메뉴를 누른 뒤 개발자 모드로 변경

이후 제어판의 프로그램 제거 - Windows 기능 켜기/끄기를 누르면 나오는 창에서 "Windows용 Linux 하위 시스템(베타)"를 선택하면 커맨드창에서 bash를 사용할 수 있다.


# 사용기, 팁

패키지 관리자를 통한 패키지 목록 업데이트는 잘 수행됨

    apt-get update
    apt-get upgrade

패치 등의 업그레이드도 잘 수행됨

bash 명령 수행시 별도의(chroot같은) 영역으로 이동하게 되는데, 다음과 같이 외부의 윈도우 디렉토리로 접근 가능

    $ cd /mnt/c/

git client의 경우 윈도우 어플리케이션은 아무래도 무겁고 느린 감이 있어서 다음과 같이 설치

그냥 git 설치시 버전이 조금 낮다.

    $ apt-get install git
    # 설치 후
    $ git --version
    $ git version 2.10.2

git 최신버전 설치 및 git clone 수행

    $ add-apt-repository ppa:git-core/ppa
    $ apt-get update
    $ apt-get install git-core
    $ git --version
    $ git version 2.10.2

openssh-server의 경우 cmd 상태창으로 계속 linux명령을 날리는 게 불편하기도 하고, 윈도우에서 간혹 ssh를 써야 할 경우도 있어서 되는지 테스트 해 봤는데, 설치가 되어 있어서 기동해봤더니 잘 되지 않음

아래와 같은 내용을 찾아서

[How can I SSH into “Bash on Ubuntu on Windows 10”?](http://superuser.com/questions/1111591/how-can-i-ssh-into-bash-on-ubuntu-on-windows-10)

다음과 같이 테스트

    apt-get remove openssh-server
    apt-get install openssh-server
    # vi /etc/ssh/sshd_config하여 아래와 같은 내용들을 확인
    PermitRootLogin no
    AllowUsers yourusername
    PasswordAuthentication yes
    UsePrivilegeSeparation no
    # 수정 후 서비스 재시작
    service ssh --full-restart

서비스는 잘 시작되나, 계정으로 로그인시 access denied 가 계속 발생함, 비번이 틀린 건 아닌거 같고

https://www.reddit.com/r/Windows10/comments/4vx5a3/enabling_openssh_remote_ssh_logins_via_bash_on/

읽어보니 ssh 서버 포트를 변경하믄 된다는 얘기가 있다. (22번에 떠 있는 ssh서버는 윈도우에서 자체적으로 띄운 것인듯..충돌해서 발생하는 문제인 것 같다.)

변경후 putty 등 ssh client로 접속하니 잘 된다.




# Docker for Windows 사용기

작성중
