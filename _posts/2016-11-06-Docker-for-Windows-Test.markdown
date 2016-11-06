---
layout: post
title: Docker for Windows Test
date: 2016-11-06 21:49:00
tags:
- Tech
- 삽질
- Docker
---

(작성중)

이전에는 Docker Toolbox 라고 해서 가상환경을 띄워 그 안에 리눅스를 설치하고 Docker를 띄우는 방식으로 윈도우에서 Docker를 사용했었으나 금년 DockerCon 이후로 mac과 windows에서도 native하게(?) docker를 사용할 수 있게 되었다.

단 Docker for Windows의 경우 OS가 Windows 10 pro 64bit 이상이어야 함.


# 설치


https://docs.docker.com/docker-for-windows/

받은 후 실행하면 별 이상없이(?) 설치된다. cmd를 띄운 후 확인해 보니 버전은 1.12.1임

tomcat을 pull하고 실행해 보았다.

    $ docker pull tomcat
    $ docker run -d --name tomcat1 -p 8080:8080 tomcat

jdk도 깔지 않은 컴퓨터 상에서 아주 깔끔하게, 빠르게 tomcat 인스턴스가 실행된다. 개발용으로 쓰기엔 아주 좋을 듯 싶다.
