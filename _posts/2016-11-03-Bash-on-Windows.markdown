---
layout: post
title: Bash on Windows 사용법
date: 2016-11-03 22:51:00
tags:
- Tech
- 삽질
---

(작성중)

윈도우 10에서 비로소 사용 가능해진 bash(Bash on Windows) 사용법


# 설치하기

이전까지 Windows에 bash를 설치하는 방법은 조금 복잡했으나, 윈도우 1주년 업데이트 후 좀 간단해진 것 같음, 우선 최신 업데이트까지 설치한 뒤,설정 > 업데이트 및 복구 창에서 개발자용 메뉴를 누른 뒤 개발자 모드로 변경

이후 제어판의 프로그램 제거 - Windows 기능 켜기/끄기를 누르면 나오는 창에서 "Windows용 Linux 하위 시스템(베타)"를 선택하면 커맨드창에서 bash를 사용할 수 있다.

# 사용기, 팁

패키지 관리자를 통한 패키지 목록 업데이트는 잘 수행됨

    apt-get update
    apt-get upgrade

패치 등의 업그레이드도 잘 수행됨

bash 명령 수행시 별도의(linux의 chroot같은) 영역으로 이동하게 되는데, 다음과 같이 외부의 윈도우 디렉토리로 접근 가능

    $ cd /mnt/c/

git client의 경우 윈도우 어플리케이션은 아무래도 무겁고 느린 감이 있어서 다음과 같이 설치

그냥 git 설치시 버전이 조금 낮다.

    $ apt-get install git
    # 설치 후
    $ git --version
      git version 1.9.1

git 최신버전 설치 및 git clone 수행

    $ add-apt-repository ppa:git-core/ppa
    $ apt-get update
    $ apt-get install git-core
    $ git --version
      git version 2.10.2

openssh-server의 경우 cmd 상태창으로 계속 linux명령을 날리는 게 불편하기도 하고, 윈도우에서 간혹 ssh를 써야 할 경우도 있어서 되는지 테스트 해 봤는데, 설치가 되어 있어서 기동해봤더니 잘 되지 않음

아래와 같은 내용을 찾아서

[How can I SSH into “Bash on Ubuntu on Windows 10”?](http://superuser.com/questions/1111591/how-can-i-ssh-into-bash-on-ubuntu-on-windows-10)

다음과 같이 실행

    # 우선 설치되어 있는 서버를 제거 후 재설치
    $ apt-get remove openssh-server
    $ apt-get install openssh-server
    # vi /etc/ssh/sshd_config하여 아래와 같은 내용들을 확인
      PermitRootLogin no
      AllowUsers yourusername
      PasswordAuthentication yes
      UsePrivilegeSeparation no
    # 수정 후 서비스 재시작
    $ service ssh --full-restart

서비스는 잘 시작되나, 계정으로 로그인시 access denied 가 계속 발생함, 비번이 틀린 건 아닌거 같고

[reddit의 다음 글](https://www.reddit.com/r/Windows10/comments/4vx5a3/enabling_openssh_remote_ssh_logins_via_bash_on/)을 읽어보니 ssh 서버 포트를 변경하믄 된다는 얘기가 있다. (22번에 떠 있는 ssh서버는 윈도우에서 자체적으로 띄운 것인듯..충돌해서 발생하는 문제인 것 같다.)

port를 2020으로 변경후 putty 등 ssh client로 접속하니 잘 된다.


cmd 상에서 다음과 같이 명령을 수행하면 bash 를 통해 명령을 실행할 수 있다.

    bash -c 'vi'

openssh-server의 경우 다음과 같이 실행 창으로 실행할 경우 창이 꺼지면서 접속이 되지 않는다.

    # win키+R 누른 후(실행 창)
    bash 'sudo service ssh start'

다음과 같이 입력하면 접속이 가능하다. 스크립트로 만든 뒤 윈도우 시작시 실행해주면 부팅시마다 openssh-server가 실행될 수 있을 것 같다.

    # win키+R 누른 후(실행 창)
    bash 'sudo /etc/init.d/ssh start'


# 결론

- 몇가지 장점

: 윈도우에서 더이상 git-client를 깔 필요가 없다. 보통 윈도우 프로그램들보다 같은 역할을 하는 리눅스 프로그램들이 가볍고 다루기 쉽다. 리눅스에만 있던 것(find,grep...)들도 윈도우에서 가상환경 없이 실행할 수 있다. 같은 shell script를 공유할 수 있다.

: 윈도우에서 ssh를 위해 별도의 도구를 깔 필요가 없다.

- 윈도우의 디폴트 ssh서버는 윈도우의 로그인 계정과 연결되며, 접속시 cmd창이 뜬다. putty 로 접속해서 dir치고 있는 모습이 좀 생경하다.(?)

- 윈도우 대비 mac의 가장 큰 장점인, 손쉬운 cli기반 개발환경, OS패키지 관리자 사용 등에 위협이 될 것 같다. 2016 젠더 맥북프로 대신에 저렴이 윈도우 노트북을 개발용 머신으로..
