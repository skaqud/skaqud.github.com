---
layout: post
title:  "Ubuntu16.04(LTS)에 Docker 설치"
date:   2016-08-15 22:22:00
tags:
- 삽질
- Tech
---

서브 노트북으로 사용중인 X201에서 Ubuntu 16.04(LTS)에 Docker를 설치하고 구성하는 과정.

# 설치방법

참고

[공식사이트-docs>Install>Ubuntu](https://docs.docker.com/engine/installation/linux/ubuntulinux/)

커널이 3.10 이상임을 확인하여야 함.

    uname -r
    3.11.0-15-generic

이후 아래와 같이 수행

    #필요한 것들 추가 설치
    sudo apt-get update
    sudo apt-get install apt-transport-https ca-certificates

    #Add the new GPG key.
    sudo apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D

/etc/apt/sources.list.d/docker.list 파일에 다음 내용 추가

    deb https://apt.dockerproject.org/repo ubuntu-xenial main

이후 다음과 같이 설치

    sudo apt-get update
    sudo apt-get install docker-engine

설치 후 다음과 같이 docker 서비스를 시작하고 확인함.

    sudo service docker start

테스트로 tomcat을 docker hub에서 pull 하여 확인

    sudo docker pull tomcat
    #명령으로 이미지를 받았는지 확인
    sudo docker images
    #이미지를 기반으로 컨테이너 시작
    sudo docker run -d -p 8080:8080 --name=test tomcat

이후 로컬의 브라우저를 통해 톰캣 디폴트 화면을 확인

    
