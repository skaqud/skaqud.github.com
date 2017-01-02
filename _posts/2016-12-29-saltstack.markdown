---
layout: post
title: saltstack
date: 2016-12-29 23:53:00
tags:
- architecture
- Tech
- Devops
---

Saltstack 설치 및 구성방법(작성중)

# 환경 준비

Vagrant를 통해 VM을 3대 띄울 예정, 1대는 master로 나머지 2대는 minion으로 구성.

base image는 Vagrant에서 바로 다운로드(ubuntu/trusty)

    VM1 - salt-master, 2cpu, 1GB ram
    VM2 - salt-minion1, 1cpu, 512MB ram
    VM3 - salt-minion2, 1cpu, 512MB ram
    각각의 OS는 ubuntu 14.04 LTS

설치는 [이 글:"SaltStack – IT automation infrastructure management tools"](http://www.yongbok.net/blog/saltstack-it-automation-infrastructure-management-tools/)을 참고하였으며, 다음과 같은 작업들을 수행할 예정

    salt-master, minion 설치 및 연결
    GUI도구 설치 및 검토(Halite, Saltpad)
    간단한 WEB-WAS-DB구성 자동설치,이중화 테스트


## Vagrant로 VM시작

IP, CPU, Memory 부분만 다르게 하여 세대의 VM을 구성함.

    #Vagrantfile
    Vagrant.require_version ">= 1.6.0"
    VAGRANTFILE_API_VERSION = "2"
    VMHOSTNAME="test1"
    VMIP="192.168.10.10"

    Vagrant.configure (VAGRANTFILE_API_VERSION) do | config |
        config.vm.box = "ubuntu/trusty64"
            config.vm.hostname = VMHOSTNAME
            # network setting
        config.vm.network :private_network, ip: VMIP

        config.vm.provider :virtualbox do | vb |
            vb.name = VMHOSTNAME
            vb.gui = false
            vb.cpus = 2
            vb.memory = 1024
         end
    end


## Saltstack 설치


host 정보를 등록

    $ vi /etc/hosts
    192.168.10.10 salt-master
    192.168.100.100 minion1
    192.168.100.200 minion2

SaltStack 저장소 등록 및 설치


    apt-get install -y python-software-properties software-properties-common
    echo deb http://ppa.launchpad.net/saltstack/salt/ubuntu `lsb_release -sc` main > /etc/apt/sources.list.d/saltstack.list
    wget -q -O- "http://keyserver.ubuntu.com:11371/pks/lookup?op=get&search=0x4759FA960E27C0A6" | apt-key add -

Saltstack 설치

    master
    apt-get update && apt-get install -y salt-master salt-syndic
    minion
    apt-get update && apt-get install -y salt-minion


Saltstack 실행

master

service salt-master start

혹은 FG 모드로
salt-master -l info


minion
salt-minion -l info

이후 서버에서 아래와 같이 승인 필요

salt-key -L
salt-key -A


몇가지 명령

    salt '* ' test.ping
    salt '* ' network.ip_addrs
    # grains 정보 가져오기
    salt '* ' grains.items
    salt '* ' grains.item host

    # 상태 가져오기
    salt '* ' status.meminfo

    # 원격 실행
    salt '* ' pkg.list_pkgs

    salt '* ' cmd.run 'echo Hello!!'

    salt '* ' service.status salt
    salt '* ' service.start salt


## Saltstack GUI 도구 설치


Halite Web UI 설치

    $ vi /etc/salt/master

아래 설정을 추가

    external_auth:   # 외부 인증 설정
      pam:
        salt:           # Halite 시스템 사용자
          - .*
          - '@runner'
          - '@wheel'

    halite:              # halite에 대한 서버 설정
      level: 'debug'
      server: 'gevent'   # cherrypy, paste, gevent 중 택일
      host: '0.0.0.0'
      port: '8080'
      cors: False


salt user 추가

    $ useradd salt
    $ passwd salt

halite 설치 및 실행

    $ git clone https://github.com/saltstack/halite /opt/halite

    $ apt-get install -y python-gevent
    $ cd /opt/halite/halite
    #시작
    $ ./server_bottle.py -d -C -l debug -s gevent


## Formular를 통한 어플리케이션 자동설치

Chef의 recipe와 같이 formula라는 이름으로 오픈소스 패키지들을 설치할 수 있는 자동 script들을 제공. Chef의 supermarket이나, Ansible의 Galaxy처럼 아직 별도의 site가 있는 건 아니며 양도 적은 편.

[공식 Home:Salt Fomular](https://github.com/saltstack-formulas)

[공식 Guide:Salt Fomular](https://docs.saltstack.com/en/latest/topics/development/conventions/formulas.html)

위의 Home에서 nginx, tomcat, mariadb 를 clone

    #nginx
    git clone https://github.com/saltstack-formulas/nginx-formula.git
    #tomcat
    git clone https://github.com/saltstack-formulas/tomcat-formula.git
    #mysql
    git clone https://github.com/saltstack-formulas/mysql-formula.git

다음 내용을 salt-master 설정에 추가 - 위의 파일들을 GitFS를 통해 minion에 공유하게 해도 되는 것 같음.

    file_roots:
      base:
        - /srv/salt
        - /srv/formulas/nginx-formula
        - /srv/formulas/tomcat-formula
        - /srv/formulas/mariadb-formula

Salt-master 재시작

이후


pillar - Saltstack에서 static한 값들을 저장하는 global 영역




# 참고

- [SaltStack – IT automation infrastructure management tools](http://www.yongbok.net/blog/saltstack-it-automation-infrastructure-management-tools/)

- [Saltstack 너란 녀석. - Grains와 Pillar (2)](http://bluese05.tistory.com/43)
