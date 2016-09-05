---
layout: post
title:  "Docker 컨테이너를 빌드하기"
date:   2016-08-21 18:30:00
tags:
- 삽질
- Tech
- Docker
---

로컬에서 docker를 통해 Java application을 Deploy하는 방법

# 빌드 시작하기

참고 : [Deploying Java applications with Docker](https://blogs.infosupport.com/deploying-java-applications-with-docker/)

참고 : [USING DOCKER TO EFFICIENTLY CREATE MULTIPLE TOMCAT INSTANCES](http://blog.trifork.com/2013/08/15/using-docker-to-efficiently-create-multiple-tomcat-instances/)

가장 단순하게, 다음과 같은 docker 파일을 생성, OS image 위에서 아래와 같은 작업을 미리 빌드시에 해 둔 새로운 이미지를 빌드함. jdk설치 등은 조금 시간이 오래 걸리기도 함, 한 번 실행시켰던 명령은 캐쉬에서 읽어와 빨리 진행된다.


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

    USER root

    COPY docker-entrypoint.sh /
    RUN chmod 755 /docker-entrypoint.sh
    ENTRYPOINT ["/docker-entrypoint.sh"]

    EXPOSE 8080
    CMD service tomcat7 start && tail -f /var/log/tomcat7/catalina.out

빌드는 다음과 같이 수행

    docker build -t ska/tomcat:0.1 .

실행은 다음과 같이 할 수 있음.

    docker run -d --name skat -p 8080:8080 ska/tomcat:0.1

하지만 위와 같이 실행하면 4,5초 후에 다음과 같이 프로세스가 죽어버림

    docker ps -a
    CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                     PORTS               NAMES
    205d633447e4        ska/tomcat:0.1      "/bin/sh -c 'service "   10 seconds ago      Exited (1) 4 seconds ago                       skat

정확히 원인을 찾을 수 없을 경우 아래와 같이 컨테이너를 시작하면서 들어가서 원인을 확인해야 함.

    #-i:interactive, -t:tty + 맨 끝 shell
    docker run -i -t --name skat -p 8080:8080 ska/tomcat:0.1 /bin/bash

Dockerfile을 통한 docker container의 경우 CMD로 실행되는 서비스 데몬과 함께 생명주기를 같이 함. 때문에 위의 경우 tomcat 데몬이 실행되고 나면 죽어버리는 것 같다. 이를 방지하기 위해 기존 CMD는 제거하고, 다음과 같이 맨 뒤에 입력해 주어야 한다.

    CMD ["catalina.sh","run"]

또한 해야 할 작업이 여러가지일 경우, 예를 들어 DB올리고 WAS올리고 WEB도 올려야 한다면, Docker의 CMD는 한 프로세스에만 걸어놓고 나머지는 entrypoint.sh을 별도로 두어 실제로 Container가 실행되고 난 후 동적으로 올려야 할 서비스를 시작하는 데 사용한다.

    USER root

    COPY docker-entrypoint.sh /
    RUN chmod 755 /docker-entrypoint.sh
    ENTRYPOINT ["/docker-entrypoint.sh"]

    EXPOSE 8080
    CMD ["catalina.sh","run"]

docker-entrypoint의 내용은 다음과 같다.

    $vi docker-entrypoint.sh
    #!/bin/bash
    set -e

    cd /usr/share/tomcat7/bin
    ./catalina.sh run

    while [ `netstat -an | grep 8080 | wc -l` -eq 0 ]
    do
            echo "Wating for tomcat7 start up"
            sleep 3;
    done

    echo 'tomcat7 start up OK..........'

    exec "$@"

실행 후 정상적으로 run 됨을 확인할 수 있다.

    $ docker ps -a           

    CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
    a2953e534aac        ska/tomcat:0.1      "/docker-entrypoint.s"   7 minutes ago       Up 7 minutes        0.0.0.0:8080->8080/tcp   skat

마지막으로 원하는 Java Application이 있으면 상단의 Dockerfile에 COPY하고 webapps 경로에 미리 복사해두면 Deploy된다.DB도 올려야 한다면 하나의 컨테이너에 올릴 수 있다.

다만, 지금은 위와 같이 사용하는 것 보다는 docker-compose를 사용하는 방법이 훨씬 더 효율적이다. 각 구성요소별로 분리(WEB,WAS,DB 등)하여 컨테이너를 구성하는 방법이 Docker의 사상에도 맞는 것 같음.
