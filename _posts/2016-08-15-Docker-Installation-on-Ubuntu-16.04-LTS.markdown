---
layout: post
title:  "Ubuntu16.04(LTS)에 Docker 설치"
date:   2016-08-15 22:22:00
tags:
- 삽질
- Tech
- Docker
---

서브 노트북으로 사용중인 X201에서 Ubuntu 16.04(LTS)에 Docker를 설치하고 구성하는 과정.

# 설치방법

참고

[공식사이트-docs>Install>Ubuntu](https://docs.docker.com/engine/installation/linux/ubuntulinux/)

커널이 3.10 이상임을 확인하여야 함.

    $uname -r
    3.11.0-15-generic

이후 아래와 같이 수행

    #필요한 것들 추가 설치
    $sudo apt-get update
    $sudo apt-get install apt-transport-https ca-certificates

    #Add the new GPG key.
    $sudo apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D

/etc/apt/sources.list.d/docker.list 파일에 다음 내용 추가

    deb https://apt.dockerproject.org/repo ubuntu-xenial main

이후 다음과 같이 설치

    $sudo apt-get update
    $sudo apt-get install docker-engine

설치 후 다음과 같이 docker 서비스를 시작하고 확인함.

    $sudo service docker start

테스트로 tomcat을 docker hub에서 pull 하여 확인

    $sudo docker pull tomcat
    Using default tag: latest
    latest: Pulling from library/tomcat
    357ea8c3d80b: Pull complete
    52befadefd24: Pull complete
    42f3df327392: Pull complete
    4e195c5e4516: Pull complete
    297a64db205e: Pull complete
    600f17c406e3: Pull complete
    30e8d8bfa9f5: Pull complete
    3be4eef33530: Pull complete
    c8da2b1886ec: Pull complete
    336408773ec8: Pull complete
    1f5bc41dc68d: Pull complete
    Digest: sha256:c3e0ecbb5844d2bf209c423fcaeddce03307107b1d486b7ef0e50f59a6829646
    Status: Downloaded newer image for tomcat:latest

    #명령으로 이미지를 받았는지 확인
    $sudo docker images
    REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
    tomcat              latest              a48852c0fc95        3 days ago          357.4 MB

    #이미지를 기반으로 컨테이너 시작
    $sudo docker run -d -p 8080:8080 --name=test tomcat
    27134fb70d48304a3d71775247e88f31394580dd484152415f382b9579e5aa40

    $sudo docker ps -a
    CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                    NAMES
    27134fb70d48        tomcat              "catalina.sh run"   8 seconds ago       Up 8 seconds        0.0.0.0:8080->8080/tcp   test


이후 로컬의 브라우저를 통해 톰캣 디폴트 화면을 확인

# 기타

사용중인 계정으로 docker 명령 실행하기 - 계정을 docker 그룹에 등록

    sudo usermod -aG docker ska

적용후 다시 로그인 해야 함

#기본명령

docker 관련 기본 명령 목록

    #이미지 조회
    $docker images
    #이미지 삭제
    $docker rmi 이미지

    #이미지 검색
    $docker search
    #이미지 가져오기
    $docker pull

    #빌드
    $docker build -t ska/myapp:1.0 .
    $docker build -t ska/java_web:1.0 .

    #컨테이너 시작
    $docker run -d --name myapp -p 8080 ska/myapp:1.0
    $docker run -i --name myapp -t ska/myapp:1.0 /bin/bash
    $docker run -d --name java_web -p 8080 ska/java_web:1.0
    $docker run -i --name java_web -t ska/java_web:1.0 /bin/bash

    # 그냥 stop했을 경우 재접속
    docker attach myapp

- -d : 데몬으로 실행, -i : interactive mode로 실행
- --name : 명칭을 부여, 안할시 자동으로 부여된다. 알아보기 쉽도록 부여하는 게 좋음(시작, 종료시 name으로 식별)
- -p : 사용할 포트, 8080:8000 일 경우 HOSTPORT:CONTAINERPORT 임.
- -t 이미지 shell : 이미지 안을 뒤질 필요가 있을 때, 해당 shell로 이미지에 접근
- -link : CONTAINER:ALIAS 형태로 다른 컨테이너와 연결, TCP/IP로 연결함.
