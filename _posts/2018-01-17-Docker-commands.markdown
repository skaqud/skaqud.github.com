---
layout: post
title: Docker commands
date: 2018-01-17 01:0:00
tags:
- Tech
- Docker
---

Docker-mastery 과정에서 정리한 Docker commands 모음(from udemy)

기존 버전과 달리 docker 관련 명령들이 docker + command + sub-command 방식으로 변경됨, 기존 명령도 동일하게 동작하나, 새로운 방식으로 사용하는 게 좋을 듯.

# basic command

    # 컨테이너 기동하기
    docker container run --publish 80:80 nginx
    # 컨테이너 기동(백그라운드=-d(--detach))
    docker container run --publish 80:80 --detach nginx
    # 컨테이너 기동(이름지정)
    docker container run --publish 80:80 --detach --name webhost nginx

    # 목록조회
    docker container ls
    # 목록조회(stop중인것까지) - 앞3문자까지 입력해도 동작, zsh이므로 자동완성을 사용해도 됨
    docker container ls -a
    docker container stop a4e

    # 로그 확인
    docker container logs webhost
    # 로그 확인(tailing)
    docker container logs -f webhost
    # 프로세스 목록
    docker container top webhost
    docker container rm a4e(docker id의 앞 세자리)
    # 기동 중이어도 삭제
    docker container rm -f a4e
    docker container run -d --name nginx nginx
    docker container run -d --name mysql -e MYSQL_RANDOM_ROOT_PASSWORD=true mysql
    docker container top mysql
    docker container inspect mysql
    docker container stats mysql

    #ssh와 유사하게 shell접근하기
    docker container run -it --name proxy nginx bash
    docker container run -it --name ubuntu ubuntu
    docker container start -ai ubuntu
    docker container exec -it --name mysql bash
    #alpine은 아주 작은 linux 배포본(bash가 없음)
    docker container run -it alpine sh

# network command

    # port 확인(outer:inner)
    docker container run -p 80:80 --name webhost -d nginx
    docker container port
    # docker network 관련 명령들
    docker network ls
    # docker0 - bridge network
    docker network inspect bridge
    docker network create my_app_net
    docker container run -d --name new_nginx --network my_app_net nginx
    # test 후 삭제(--rm)
    docker container run --rm --it ubuntu:14.04 bash

    #net-alias 주기
    docker container run -d --net dude --net-alias search elasticsearch:2

# image, volume command

    #이미지 확인
    docker image inspect nginx를
    #히스토리 확인
    docker image history mysql

    #add named volume
    -v mysql-db:/var/lib/mysql
    #create volumn
    docker volumn create --help
    #bind mounting(add pwd)
    -v $(pwd):/usr/share/nginx/html


# Docker-compose

    docker-compose up -d
    docker-compose down
    #볼륨까지 제거
    docker-compose down -v

    docker-compose logs
    docker-compose logs -f

    docker-compose ps
