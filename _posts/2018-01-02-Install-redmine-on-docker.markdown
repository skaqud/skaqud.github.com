---
layout: post
title: Redmine을 Docker로 설치
date: 2018-01-02 22:00:00
tags:
- Tech
- Docker
---

Docker 상에서 Redmine을 설치하고 설정하는 방법을 정리, 의외로 삽질을 많이 하게 됨.

# 이미지 선택

Docker hub에서 redmine으로 검색하면 많은 이미지들이 뜸, 가급적이면 별이 많은 이미지나, redmine쪽에서 공식적으로 올려놓은 이미지를 이용, 사용자들의 custom이미지들이 많으나, 문서화 된 내용을 보고 설정(이메일,테마,플러그인 설치 등)이 어렵지 않은지 보고 이용해야 함. 여기서는 [Redmine 공식이미지](https://hub.docker.com/_/redmine/)를 이용한다.

# Redmine 설치(on docker)

별도로 설치하는 방법보다 간단함.
우선은 db를 먼저 설치한다. 이미 설치되어 있는 db예 database를 만들고, 계정을 설정하여 사용하거나, mysql 자체도 docker로 띄워 link시켜 사용할 수 있다.

별도로 사용할 경우 다음과 같이 database를 만들고 계정을 설정한다.(mysql기준)

    #설치 형태에 따라 local과 %로 권한을 다 줘야 함.(권한주의)
    CREATE DATABASE redmine CHARACTER SET utf8mb4;
    CREATE USER 'redmine'@'localhost' IDENTIFIED BY 'redmine123!';
    GRANT ALL PRIVILEGES ON redmine.* TO 'redmine'@'localhost';
    CREATE USER 'redmine'@'%' IDENTIFIED BY 'redmine123!';
    GRANT ALL PRIVILEGES ON redmine.* TO 'redmine'@'%';
    flush privileges;

아니면 다음과 같이 mysql을 docker에 띄울 수 있다. 아직 db까지는 많이들 올리지 않음.

    docker run -d --name red-mysql -e MYSQL_ROOT_PASSWORD=redmine123! -e MYSQL_DATABASE=redmine mysql

이후 redmine을 다음과 같이 띄운다.

별도로 사용할 경우(env에 해당하는 값들은 설정된 db 값들을 넣어준다.)

    docker run -d --name redmine2 --env='REDMINE_DB_MYSQL=192.168.0.10' \
    --env='REDMINE_DB_USERNAME=redmine' \
    --env='REDMINE_DB_PASSWORD=redmine123!' \
    --env='REDMINE_DB_DATABASE=redmine' \
    --env='REDMINE_PLUGINS_MIGRATE=true' \
    -p 3333:3000 redmine

docker로 띄울 경우

    docker run -d --name redmine2 --link red-mysql:mysql -p 3333:3000 redmine

이후 localhost의 3333 포트로 접근하여 서비스를 확인한다.(http://localhost:3333/)

# Redmine 설정

별도로 설치할 경우보다 좀 번거롭다. 파일을 직접 수정하는 게 아니라, docker 명령의 옵션을 통해 입력하거나, volume을 mount하여 별도의 명령을 실행해야 함.

### 이메일 설정

공식 이미지의 경우 smtp 설정을 별도 파일(config.yml)에서 해야 하는데, 아직 외부 환경설정 등으로 지원하지 않는 것 같다. [bitnami의 이미지](https://hub.docker.com/r/bitnami/redmine/)를 쓰면 쉽게 설정 가능하다.

### 플러그인,테마 디렉토리 마운트

다음과 같이 옵션을 통해 이용하여 외부:컨테이너내부 를 마운트한다. 기동시 추가

    -v /home/ska/dev/docker/redmine/data/files:/usr/src/redmine/files \
    -v /home/ska/dev/docker/redmine/data/plugins:/usr/src/redmine/plugins \

### 플러그인 설치

원하는 플러그인을 zip파일로 받아 압축을 풀거나, git clone으로 직접 다운로드 한다. /usr/src/redmine/plugins상에 올라온 플러그인 들이 인식된다. docker가 정상적으로 올라가지 않는다면 it옵션으로 기동 로그를 확인한다.


# Redmine 이미지 빌드

docker의 경우 이미지 작성자가 별도로 설정해 놓은 옵션이나 환경변수 이외에 별도의 작업을 프로세스 기동 전,후에 수행하려면 별도로 이미지를 빌드해줘야 한다. docker hub에 올라온 이미지들의 경우 해당 페이지에서 Dockerfile을 어렵지 않게 구할 수 있다.

Dockerfile이 있는 곳에서 다음과 같이 빌드

    docker build --tag ska/redmine:0.1 .

Dockerfile의 내용은 생각보다 어렵지 않으나, 설치 과정이 복잡할 경우 선/후작업이 문제가 될 수 있음. 원하는 곳에 원하는 명령을 넣은 뒤 다시 빌드해 본다.

빌드한 이미지로 redmine을 시작해 본다.

    docker run -d --name redmine2 --link red-mysql:mysql -p 3333:3000 \
    -v /home/ska/dev/docker/redmine/data/files:/usr/src/redmine/files \
    -v /home/ska/dev/docker/redmine/data/plugins:/usr/src/redmine/plugins \
    ska/redmine:0.1
