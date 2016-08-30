---
layout: post
title:  "Docker 컨테이너를 빌드하기"
date:   2016-08-21 18:30:00
tags:
- 삽질
- Tech
- Docker
---

로컬에서 docker를 통해 Java application을 Deploy하는 방법(작성중)

로컬에서 docker를 통해 jekyll을 빌드하고 실행하는 방법.(보류)

- rvm으로 설치시 cpp,gcc,g++ 설치를 요청하는데, dependancy 문제가 있음.

# 빌드 시작하기

참고 : [Deploying Java applications with Docker](https://blogs.infosupport.com/deploying-java-applications-with-docker/)

참고 : [USING DOCKER TO EFFICIENTLY CREATE MULTIPLE TOMCAT INSTANCES] (http://blog.trifork.com/2013/08/15/using-docker-to-efficiently-create-multiple-tomcat-instances/)

가장 단순하게, 다음과 같은 docker 파일을 생성


    FROM ubuntu:14.04

    #daum의 repository를 사용하지 않을 경우, 중간에 apt-get을 통한 설치가 끊김.
    RUN echo "deb http://ftp.daumkakao.com/ubuntu trusty main universe" > /etc/apt/sources.list
    #RUN echo "deb http://archive.ubuntu.com/ubuntu trusty main universe" > /etc/apt/sources.list

    RUN apt-get update &&  apt-get -y upgrade
    # Add oracle java 7 repository
    RUN apt-get -y install software-properties-common
    RUN add-apt-repository ppa:webupd8team/java
    RUN apt-get -y update
    # Install basic packages
    RUN apt-get -y install git curl build-essential

    # Accept the Oracle Java license - sun-jdk설치시 라이센스 동의부분
    RUN echo "oracle-java7-installer shared/accepted-oracle-license-v1-1 boolean true" | debconf-set-selections

    # Install Oracle Java
    RUN apt-get -y install oracle-java7-installer
    # Install tomcat
    RUN apt-get -y install tomcat7
    RUN echo "JAVA_HOME=/usr/lib/jvm/java-7-oracle" >> /etc/default/tomcat7
    EXPOSE 8080

빌드는 다음과 같이 수행

    docker build -t ska/tomcat:0.1 .

실행은 다음과 같이 할 수 있음.

    docker run -d --name skat -p 8080:8080 ska/tomcat:0.1
