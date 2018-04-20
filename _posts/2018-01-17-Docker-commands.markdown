---
layout: post
title: Docker commands
date: 2018-01-17 01:10:00
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


# docker-compose

    # 시작(설정파일 필요)
    docker-compose up
    # 기동(background로)
    docker-compose up -d
    # 기동(background로, 특정 서비스만)
    docker-compose up -d mysql
    # 기동(background로, 특정 서비스만, scale설정)
    docker-compose up -d scale mysql=2

    # 종료
    docker-compose down
    #볼륨까지 제거
    docker-compose down -v

    # 로그 확인
    docker-compose logs
    docker-compose logs -f

    # docker container 상태 확인
    docker-compose ps

    # 생성된 이미지까지 삭제
    docker-compose down -rmi local(all)

    # 프로젝트 지정
    docker-compose -p myproject



# docker swarm mode

    #정보 조회 - 초기에는 비활성화 되어 있음
    docker info
      Swarm: inactive

    # swarm 활성화
    docker swarm init

    # node 조회
    docker node ls
    ID                            HOSTNAME            STATUS              AVAILABILITY        MANAGER STATUS
    57d08vdds8n1tn6c1l867iaft *   ska-Inspiron-7373   Ready               Active              Leader

    # 서비스 생성
    docker service create alpine ping 8.8.8.8

    # 서비스 조회
    docker service ls
    ID                  NAME                MODE                REPLICAS            IMAGE               PORTS
    onkpl2ime0j6        loving_fermi        replicated          1/1                 alpine:latest

    # 서비스 업데이트(복제)
    docker service update onkpl2ime0j6 --replicas 3

    # 다시 서비스 조회
    docker service ls
    ID                  NAME                MODE                REPLICAS            IMAGE               PORTS
    onkpl2ime0j6        loving_fermi        replicated          3/3                 alpine:latest
     ska  ~/git/udemy-docker-mastery/compose-sample-3   master ●  docker service ps loving_fermi
    ID                  NAME                IMAGE               NODE                DESIRED STATE       CURRENT STATE            ERROR               PORTS
    yf8kir63pbdf        loving_fermi.1      alpine:latest       ska-Inspiron-7373   Running             Running 2 minutes ago
    rhzj1kxgy6up        loving_fermi.2      alpine:latest       ska-Inspiron-7373   Running             Running 33 seconds ago
    znf26wmdspkf        loving_fermi.3      alpine:latest       ska-Inspiron-7373   Running             Running 33 seconds ago


## Multinode에서 swarm을 테스트하는 방법

- 기존에 알던 것 이외에 docker-machine을 이용한 방법이 있음.

- docker-machine - Docker가 설치된 linux vm을 띄울 수 있는 명령, driver를 local,cloud등으로 두어 어디서든 docker를 띄울 수 있도록 도와준다.(포지션이 vagrant와 비슷한 것 같음.)


overlay network - swarm 상에 subnet을 만들어 같은 network상에 컨테이너들을 생성할 수 있음.

routing mesh - swarm상에서 들어오는 패킷을 routing, virtual ip로 network을 묶을 수 있음. stateless load balancer(tcp layer)

watch 명령 - 주기적으로 결과를 보여줌.
watch docker service ls

## 기타

docker deamon에 보안 적용 : --tlsverify
  - 기본 설치시 보안 연결이 적용되어 있지 않으며, 실 운영시에는 적용하여 데몬에 TLS, 도커 클라이언트는 인증된 것만 사용하도록 적용해야 함

docker stats : 실행중인 모든 컨테이너의 자원 사용량

스웜/스웜 모드 : 스웜(에이전트,코디네이터 필요), 스웜모드(도커에 내장됨)

rancher-server 설치 및 테스트 :

rancher server 설치 및 테스트해 보기

    docker container run -d \
    --restart=unless-stopped -p 8080:8080 \
    --name=rancher-server rancher/server
