---
layout: post
title: gogs 설치 및 설정
date: 2017-08-18 01:42:00
tags:
- Tech
---


# gogs 설치 및 환경설정(nginx-gogs-mysql)

참고 
- https://gogs.io/docs/installation
- https://gogs.io/docs/intro/faqs

## 연결할 DB설정 - database 생성(mysql)

CREATE DATABASE gogs CHARACTER SET utf8mb4
--계정 생성 및 권한부여
create user gogs@localhost identified by 'gUser2953!';
grant all privileges on gogs.* to gogs@localhost;
flush privileges;

## 설치

    #바이너리 다운로드 및 설치
    $ wget https://dl.gogs.io/0.11.29/linux_amd64.tar.gz
    $ tar zxvf linux_amd64.tar.gz
    $ cd gogs
    $ ./gogs web

    이후 http://localhost:3000 에 브라우저로 접속하여 초기 설정


## nginx 설정, 연결

    server {
       listen 80;
       server_name www.abc.com;

        location /gogs/ {
            proxy_pass http://localhost:3000/;
        }
    }

nginx를 통해 push 시 413 Request Entity Too Large 에러 발생해서 다음과 같이 수정

nginx.conf 파일 server 영역 안에 추가
- client_max_body_size 20M; 추가


## gogs의 설정 변경
    
   custom/conf/app.ini 파일의 다음 부분 수정
   ROOT_URL         = http://www.abc.com/gogs 

   이후 재기동
   nohup ./gogs web &


