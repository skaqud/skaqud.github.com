---
layout: post
title: saltstack(1)
date: 2016-12-29 23:53:00
tags:
- architecture
- Tech
- Devops
---

Saltstack 설치 및 구성방법

* TOC
{:toc}
---

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
    간단한 WEB-WAS-DB구성 자동설치,이중화 테스트 state 작성 - 다음


## Vagrant로 VM시작

IP, CPU, Memory 부분만 다르게 하여 세대의 VM을 구성함.

    #vi Vagrantfile
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

OS Repository(LTS)의 버전은 적어도 꽤 오래전 버전임, 최신버전이 필요할 경우 별도의 os repository를 등록 후 설치해야 하거나, salt document를 참고

    #master
    $ apt-get update && apt-get install -y salt-master salt-syndic
    #minion
    $ apt-get update && apt-get install -y salt-minion

최신 버전 설치를 위해서는 다음과 같이 수행

ubuntu의 경우

    $ apt-get install -y python-software-properties software-properties-common
    $ wget -O - https://repo.saltstack.com/apt/ubuntu/16.04/amd64/latest/SALTSTACK-GPG-KEY.pub | sudo apt-key add -
    $ vi /etc/apt/sources.list.d/saltstack.list
    deb http://repo.saltstack.com/apt/ubuntu/14.04/amd64/latest trusty main

이후 apt-get update 명령 한 번 수행 후 설치는 동일

    apt-get update
    apt-get install -y salt-master salt-syndic
    apt-get install -y salt-minion

CentOS의 경우

    $ sudo yum install https://repo.saltstack.com/yum/redhat/salt-repo-latest-1.el7.noarch.rpm

    $ sudo yum install salt-master
    $ sudo yum install salt-minion

버전을 확인해서 목표로 하는 버전이 맞는지 확인

    $ salt --version
    salt 2016.11.1 (Carbon)

수동설치 참고 - https://repo.saltstack.com/#ubuntu

Saltstack 실행

    #master
    #서비스로
    $ service salt-master start
    #혹은 FG 모드로
    $ salt-master -l info

    #minion(실행 전 master 설정을 해야 함 /etc/salt/minion 파일)
    $ salt-minion -l info
    $ service salt-minion start

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
    # 일반적으로 작성한 state를 실행
    # - state_name.sls 혹은 state_name/init.sls가 실행
    salt '* ' state.apply state_name
    # 아무것도 주지 않으면 최상위의 top.sls가 실행된다.
    salt '* ' state.apply


## Saltstack GUI 도구 설치

참고의 wiki를 따라가보면, 오픈소스 중에 유망(?)한 건 두가지인듯 싶다. Halite,Saltpad

### Halite 설치

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

실제로 master, minion, 실행 이력, grains등이 조회되고, 웹을 통해 실행도 가능하다. 하지만, 생각처럼 기능이 많지 않아 관리도구로 사용하기 부적절해 보인다.

### Saltpad 설치

아직 정상동작 확인은 못함, nginx 설정에서 api url로 요청시 301에러가 난다.(작성중)

설치는 [github홈](https://github.com/Lothiraldan/saltpad/blob/master/docs/installation/nginx-across-internet-cors.md)을 참고, 조금 복잡하게 되어 있는데, salt-api를 써서 salt와 통신하고, nginx를 통해 웹서비스를 하는 방식인 듯 하다. 계정은 동일하게 OS걸 사용.

    # master 설정에 추가
    $ vi /etc/salt/master
    rest_cherrypy:
      port: 8000
      host: 127.0.0.1
      disable_ssl: True

    # salt-api 설치 후 다음과 같이 확인
    $ curl -i http://localhost:8000/
    $ curl -i -H accept=application/json -d username=salt -d password=salt -d eauth=pam http://localhost:8000/login

    # saltpad 다운로드
    $ wget https://github.com/Lothiraldan/saltpad/releases/download/v0.3.1/dist.zip
    # 적절한 곳에 압축을 풀고(계정도 만들어야 하니 /home/salt)
    # 설정파일을 수정
    $ cat /home/salt/saltpad/static/settings.json
    {
        "API_URL": "localhost:8000",
        "SECURE_HTTP": false,
        "FLAVOUR": "rest_tornado"
    }
    # nginx를 설치 후 다음과 같이 설정을 추가
    server {
    listen 80 default_server;
    listen [::]:80 default_server ipv6only=on;

    # Saltpad specific
    root /home/salt/saltpad/;
    index index.html;

    server_name 192.168.10.10;

    location / {
        try_files $uri /index.html;
    }

    # Salt-api specific
    location /api/ {
        proxy_pass       http://localhost:8000/;
        proxy_set_header Host      $host;
        proxy_set_header X-Real-IP $remote_addr;
    }

    https를 추가하기 위해서는 let's encrypt의 인증서를 받아 https로 설치하면 될 듯
    다만, 위의 /api/ 경로 호출시 301에러가 발생함


# 참고

- [SaltStack – IT automation infrastructure management tools](http://www.yongbok.net/blog/saltstack-it-automation-infrastructure-management-tools/)

- [Saltstack 너란 녀석. - Grains와 Pillar (2)](http://bluese05.tistory.com/43)

- [Saltstack documentation](https://docs.saltstack.com/en/latest/) - 처음에 개념을 잡기 좀 힘들었는데, 중간부분에 Tutorial을 따라 해 보면서 감을 잡을 수 있었음.

- [Github-Saltpad](https://github.com/Lothiraldan/saltpad)
