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

다음과 같은 환경을 테스트로 구성할 예정, WEB과 WAS는 이중화 되어 있고, DB의 경우 비용이 좀 많이 들어가게 되어(너무 복잡해진다...) 우선은 대상에서 제외.

    WEB1(nginx) --- WAS1(tomcat7-petclinic<war>)
                 X                               > DB(MariaDB)
    WEB2(nginx) --- WAS2(tomcat7-petclinic<war>)            


참고 - [A sample Docker workflow with Nginx, Node.js and Redis](http://anandmanisankar.com/posts/docker-container-nginx-node-redis-example/)

참고2 - [How to use Docker Compose to run complex multi container apps on your Raspberry Pi](http://blog.hypriot.com/post/docker-compose-nodejs-haproxy/)


# 준비

## docker-compose 설치

Docker 설치에 비해 Docker-compose의 설치는 정말 간단하다.(Docker engine이 이미 설치되어 있어야 한다...Docker 설치는 예전에는 간단하지 않았음.)

[공식 Document Page](https://docs.docker.com/compose/install/)

    $ curl -L https://github.com/docker/compose/releases/download/1.8.0/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose

    #Permission denied 오류가 /usr/local/bin/docker-compose에서 나면
    #sudo -i 후 다시 실행

    $ chmod +x /usr/local/bin/docker-compose
    $ docker-compose --version
    docker-compose version 1.8.0, build f3628c7

## WAR 준비

WAS에 deploy할 petclinic war를 미리 준비한다. 해당 프로젝트를 clone한 뒤 이클립스 혹은 Maven으로 빌드한다. 필요한 파일은 war.

    git clone https://github.com/spring-projects/spring-petclinic.git

위의 프로젝트를 clone한 뒤 이클립스에서든 그냥 maven을 이용하든 war파일을 생성한다.

# 구성

기본적으로 web, was, db를 위한 컨테이너 이미지가 하나씩은 필요함. 다음과 같이 공식 이미지를 pull 받는다.

    $ docker pull nginx
    $ docker pull tomcat
    $ docker pull mariadb

이후 생성한 이미지를 다음과 같은 docker-compose 설정파일을 통해 시작한다. 편의상 필요한 설정들은 conf, 필요한 binary나 설치파일들은 files 밑에 넣도록 했다.

    #docker-compose.xml 파일 내용
    web:
        image: nginx
        ports:
            - "80:80"
        links:
            - petclinic:petclinic
            - petclinic2:petclinic2
        volumes:
            - ./conf/nginx.conf:/etc/nginx/nginx.conf
    petclinic:
        image: tomcat
        ports:
            - "8080"
        links:
            - db
        volumes:
            - ./files/petclinic.war:/usr/local/tomcat/webapps/petclinic.war
    petclinic2:
        image: tomcat
        ports:
            - "8080"
        links:
            - db
        volumes:
            - ./files/petclinic.war:/usr/local/tomcat/webapps/petclinic.war
    db:
        image: mariadb
        ports:
            - "3306:3306"
        environment:
            MYSQL_ROOT_PASSWORD: petclinic
            MYSQL_DATABASE: petclinic

port에 두개의 포트가 지정되어 있으면 내부:외부를 의미, links에서 이용되는 이름들은 컨테이너간 host명이라고 봐도 될 것 같다. 실제로 was에서 db연결시 해당 이름으로 연결된다.

nginx는 이중화된 was의 컨테이너를 바라볼 수 있도록 아래의 설정파일을 nginx컨테이너에 복사하여 적용되도록 한다.

    #nginx.conf 파일 내용

    user  nginx;
    worker_processes  4;

    error_log  /var/log/nginx/error.log warn;
    pid        /var/run/nginx.pid;

    events {
        worker_connections  1024;
    }

    http {
        include       /etc/nginx/mime.types;
        default_type  application/octet-stream;
        client_max_body_size 4000m;


        log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                          '$status $body_bytes_sent "$http_referer" '
                          '"$http_user_agent" "$http_x_forwarded_for"';


        access_log  /var/log/nginx/access.log  main;


        sendfile        on;
        #tcp_nopush     on;


        keepalive_timeout  65;
        gzip  on;


        upstream pet-app {
              least_conn;
              server petclinic:8080 weight=10 max_fails=3 fail_timeout=30s;
              server petclinic2:8080 weight=10 max_fails=3 fail_timeout=30s;
        }


        server {
              listen 80;

              location / {
                proxy_pass http://pet-app;
                proxy_http_version 1.1;
                proxy_set_header Upgrade $http_upgrade;
                proxy_set_header Connection 'upgrade';
                proxy_set_header Host $host;
                proxy_cache_bypass $http_upgrade;
              }
        }
    }


참고 - 실제파일 경로

