---
layout: post
title: salt-ssh 설치 및 사용법
date: 2019-07-07 24:00:00
tags:
- Tech
- Saltstack
- Devops
---

saltstack는 강력하고 편리하기는 하나, 사용을 위해서는 반드시 minion 이라는 agent를 설치해야 한다. 이 문서에서는 minion 없이 salt-ssh만을 통해 salt state들을 전송하는 방법을 설명한다.

우선은 테스트할 서버를 마련한다. vagrant를 통해 동일한 vm2대를 생성(ubuntu 18.04)하고 편의상 한대는 master, 나머지 한대는 minion이라고 한다. master는 명령을 내리는 서버이며, minion은 테스트 대상 서버가 된다.

## 최신 버전의 salt-ssh 설치 

https://repo.saltstack.com/#ubuntu 을 참고하여 master에 salt-ssh를 설치한다.

    wget -O - https://repo.saltstack.com/apt/ubuntu/18.04/amd64/latest/SALTSTACK-GPG-KEY.pub | sudo apt-key add -

다음 내용을 /etc/apt/sources.list.d/saltstack.list 에 저장한다.

    deb http://repo.saltstack.com/apt/ubuntu/18.04/amd64/latest bionic main

    apt update
    apt install salt-ssh

다른 툴을 원한다면 아래와 같이 설치해도 좋음.

    apt install salt-master
    apt install salt-minion


## 사전작업

ssh를 통해 명령을 전달하기 위해 ssh key(openssh의 public key)를 명령을 내릴 곳에 등록해줘야 한다.

우선은 master서버의 roster파일에 명령을 내릴 대상의 정보를 등록한다.

    vi /etc/salt/roster
    minion:
    host: 192.168.100.101
    user: vagrant
    sudo: True

master서버에서 minion을 향해 키를 등록한다.

    ssh-copy-id -i /etc/salt/pki/master/ssh/salt-ssh.rsa.pub vagrant@192.168.100.101

그러나, 아무리 등록하려고 해도 다음과 같은 에러가 발생하며, 잘 되지 않음
    
    vagrant@192.168.100.101: Permission denied (publickey).

아래와 같이 대상서버의 /etc/ssh/sshd_config를 수정

    vi /etc/ssh/sshd_config
    # 다음 부분만 수정해준뒤
    # PasswordAuthentication no -> yes로

service ssh restart 로 재시작한 뒤, 다시 키를 등록하면 등록됨
    
    ssh-copy-id -i /etc/salt/pki/master/ssh/salt-ssh.rsa.pub vagrant@192.168.100.101

## 실행 및 확인

이후 아래와 같이 명령을 실행할 겸, salt formula를 전송하기 위해 python을 minion에 설치한다. 이후 명령 수행을 위해 반드시 설치 필요함.

    salt-ssh minion -r 'sudo apt update;sudo apt install -y python-minimal'

아래와 같이 일반 쉘 명령도 실행할 수 있다.

    root@master:~# salt-ssh minion -r 'pwd'
    minion:
        ----------
        retcode:
            0
        stderr:
        stdout:
            /home/vagrant

기존에 만들어 놓은 salt formula들을 다음경로에 맞게 넣은 후 실행해 본다.

    #formula 위치
    #/srv/saltstack, /srv/pillar

아래와 같이 실행하여 설치를 확인한다.

    salt-ssh '*' state.apply nginx
