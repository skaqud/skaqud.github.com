---
layout: post
title:  "Docker Compose로 HA환경 구성하기"
date:   2016-09-06 22:48:28
tags:
- 삽질
- Tech
- Docker
---

Docker compose로 간단한 HA환경을 구성해 보는 방법(작성중)

요즈음은 대부분 서비스를 적당한 Size로 구성하고 배치하여 MicroService화 하는 방법이 대세가 되었다. Dockerfile의 경우 실제 서비스를 구성하기 위해서는 손이 좀 더 가게 되는데, Docker Compose를 이용하면 Multi-node환경에서 좀 더 편하게 서비스를 구성하고 실행할 수 있다.

다음과 같은 환경을 테스트로 구성할 예정, WEB과 WAS는 이중화 되어 있고, DB의 경우 비용이 좀 많이 들어가게 되어 우선은 대상에서 제외.

WEB1(nginx) --- WAS1(tomcat7-petclinic<war>)
             X                               > DB(MariaDB)
WEB2(nginx) --- WAS2(tomcat7-petclinic<war>)            


# 준비

Docker 설치에 비해 Docker-compose의 설치는 정말 간단하다.(Docker engine이 이미 설치되어 있어야 한다.)

[공식 Document Page](https://docs.docker.com/compose/install/)

    $ curl -L https://github.com/docker/compose/releases/download/1.8.0/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose

    #Permission denied 오류가 /usr/local/bin/docker-compose에서 나면
    #sudo -i 후 다시 실행

    $ chmod +x /usr/local/bin/docker-compose
    $ docker-compose --version
    docker-compose version 1.8.0, build f3628c7

WAS에 deploy할 petclinic war를 미리 준비한다.

# 구성

기본적으로 web, was, db를 위한 컨테이너 이미지가 하나씩은 필요
