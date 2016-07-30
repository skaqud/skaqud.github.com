---
layout: post
title:  "자주 사용하는 명령들"
date:   2016-07-24 12:21:15 +0700
categories: reuse
---

자주 사용하는 명령들의 모음

## Linux

### 기타 명령들

tar명령

tar.gz, tgz 등은 단순히 묶을 때 czvf, 풀때 xzvf 만 기억하면 될 듯

    c : 압축
    z : tar 압축후 gzip압축
    v : verbose 압축과정을 출력
    p : 소유권등 퍼미션을 그대로 유지
    f : 내가 지정한 파일명으로 압축
    압축하기
    tar czvpf filename.tar.gz public_html/
    압축풀기
    tar xzvpf filename.tar.gz public_html/

find명령

    #find공백이 들어간 파일명 검색
    find ./ -name "* *"
    # 수정한지 20일 이상된 파일만 삭제 ( -exec rm {} \; )
    find . -mtime +20 -type f -ls -exec rm {} \;
    # 확장자가 .htm* .gif, .js, .css 인 것만 퍼미션을 644(rw-r--r--)로
    find . -name "*.htm*" -o -name "*.gif" -o -name "*.js" -o -name "*.css" -exec chmod 644 {} \;
    # 파일 크기가 300KB 이상( -size +300k )인 파일만
    find . -size +300k -ls
    #*..


### 참고가 될 Shell Script

    # Mysql service up check
    while [ `netstat -an | grep 3306 | wc -l` -eq 0 ]
    do
            echo "Wating for Mysql start up"
            sleep 3;
    done
    echo "Mysql is UP!"

    # Postgresql service up check
    while [ `netstat -an | grep 5432 | wc -l` -eq 0 ]
    do
            echo "Wating for Postgresql start up"
            sleep 3;
    done
    echo "Postgresql is UP!"


### 상황별 해결방법

SSH로그인자동화(공개키를 이용한)

    키 생성(있을 경우 통과)
    ssh-keygen
    타겟 서버로 복사
    scp ~/.ssh/id_rsa.pub vagrant@192.168.33.12:
    허용 키 목록에 등록(해당 서버에 접속하여)
    cat ~/id_rsa.pub >> ~/.ssh/authorized_keys
    권한 설정
    chmod 700 ~/.ssh
    chmod 644 ~/.ssh/authorized_keys
    위의 작업을 해 준 계정에만 해당됨.

OS 버전 확인

    ubuntu
    cat /etc/issue
    lsb_release -a

위와 같이 해서 안될 경우 다음과 같이 수행

    cat /etc/*release
    #*...편집용comment


저장소(Repository)서버 변경(ubuntu)

    sudo vi /etc/apt/sources.list
    :%s/archive.ubuntu.com/ftp.daumkakao.com/g
    :%s/security.ubuntu.com/ftp.daumkakao.com/g
    :%s/extras.ubuntu.com/ftp.daumkakao.com/g
    apt-get update


시스템 시작시 자동으로 시작될 script

    /etc/rc.local 하위에 추가하면 됨

설치된 패키지 조회 및 제거(ubuntu)

    dpkg --get-selections | grep jdk

RPM명령(CentOS,RHEL계열)

    #설치된 rpm 조회
    rpm -qa | grep chef
    #(위에서 이름을 확인 후)설치된 rpm 삭제
    rpm -e chef-12.4.1-1.el7.x86_64
    rpm 설치
    rpm -ivh chef-12.5.1-1.el7.x86_64.rpm
    rpm -Uvh chef-12.5.1-1.el7.x86_64.rpm


### 운영시 유용한 명령들

    * 로그 크기 확인하기(일정 크기가 넘는 로그파일 확인)
    du -k | awk '{if (length($1) > 5) print}'
    * 특정 계정에 속한 프로세스 kill 하기
    ps -ef | grep 계정 | grep -v grep | awk '{print $2}' | xargs kill -9


## 그밖의 도구들

### Docker

빌드

    docker build -t ska/myapp:1.0 .
    docker build -t ska/java_web:1.0 .

실행

    docker run -d --name myapp -p 8080 ska/myapp:1.0
    docker run -i --name myapp -t ska/myapp:1.0 /bin/bash
    docker run -d --name java_web -p 8080 ska/java_web:1.0
    docker run -i --name java_web -t ska/java_web:1.0 /bin/bash

    #사용하지 않는 컨테이너 확인
    docker ps -a | grep Exited | awk '{print $1}'
    #사용하지 않는 컨테이너 전부 삭제
    docker rm $(docker ps -a | grep Exited | awk '{print $1}')
    #사용하지 않는 이미지 확인
    docker images | grep none | awk '{print $3}'
    #사용하지 않는 이미지 전부 삭제
    docker rmi $(docker images | grep none | awk '{print $3}')


### GIT

    git add ./*
    git commit -a -m "commit message"
    git push origin master

    git config --global user.name "...."
    git config --global user.email ......

    git config http.proxy http://xx.xx.xx.xx:8888


### MySQL

    DB생성
    CREATE SCHEMA ReviewDB DEFAULT CHARACTER SET utf8 ;
    계정 생성
    create user 'gerrit'@'%' identified by 'gerrit123';
    grant all privileges on *.* to 'gerrit'@'%';
    create user 'gerrit'@'localhost' identified by 'gerrit123';
    grant all privileges on *.* to 'gerrit'@'localhost';
    flush privileges;

    Drop schema ReviewDB cascade;


### JDK

tar로 sun jdk 설치시 - apt-get 등으로는 sun-jdk를 바로 설치하기 좀 까다로움

    wget ......install_file_path/jdk-8u66-linux-x64.gz
    tar zxvf jdk-8u66-linux-x64.gz

    -ubuntu
    update-alternatives --install /usr/bin/javac javac /opt/jdk1.8.0_66/bin/javac 1
    update-alternatives --install /usr/bin/java java /opt/jdk1.8.0_66/bin/java 1
    update-alternatives --install /usr/bin/javaws javaws /opt/jdk1.8.0_66/bin/javaws 1


### Python

    바로 활용할 수 있는 단순 웹 서버(보통 python은 다 설치되어 있음)
    #run SimpleHTTPServer
    python -m SimpleHTTPServer 8080


### Screen 명령정리

예전 터미널 상에서의 screen 명령보다는 tmux를 쓰거나 terminator 등 멀티화면 지원하는 ssh client를 쓰는 게 나음.
tmux의 경우 사용법이 그나마 간단하고, 별도의 GUI를 가지고 있지 않아서 터미널상에서 바로 쓸 수 있는 멀티Screen도구일 것 같음.

    #설치
    apt-get install tmux
    #실행(실행시 아래 상태창이 생김)
    tmux
    # 새 창띄우기 - 순서대로 0부터 번호가 부여된 창이 생성됨
    Ctrl+B 누르고 C
    # 창 이동
    Ctrl+B 누르고 숫자(화면번호)
    # 같은 화면을 나눠서 쓸 경우
    Ctrl+B 누르고 화살표
    # 화면 나누기(가로)
    Ctrl+B 누르고 =
    # 화면 나누기(세로)
    Ctrl+B 누르고 %


## Proxy 설정

    #env에 추가(wget,curl 등 cli 기반 도구의 경우)
    export http_proxy="http://xx.xx.xx.xx:8888"
    export https_proxy="http://xx.xx.xx.xx:8888"

    #proxy setting(로그인쉘에 추가-bash의 경우)
    echo 'export http_proxy="http://xx.xx.xx.xx:8888"' >> ~/.bash_profile
    echo 'export https_proxy="https://xx.xx.xx.xx:8888"' >> ~/.bash_profile

    #apt proxy setting(ubuntu 패키지 관리자)
    echo 'Acquire::http::proxy "http://xx.xx.xx.xx:8888/";' >> /etc/apt/apt.conf
    echo 'Acquire::https::proxy "http://xx.xx.xx.xx:8888/";' >> /etc/apt/apt.conf

    #yum proxy setting(centos 패키지 관리자)
    echo 'proxy=http://xx.xx.xx.xx:8888' >> /etc/yum.conf

    #WINDOWS일 경우
    set HTTP_PROXY="http://xx.xx.xx.xx:8888"
    set HTTPS_PROXY="http://xx.xx.xx.xx:8888"

    #git(gitconfig) 설정
    git config http.proxy http://xx.xx.xx.xx:8888

    #docker proxy 설정
    참고 - http://nknu.net/running-docker-behind-a-proxy-on-ubuntu-14-04/

    /etc/defaults/docker
    파일 수정
    export http_proxy="http://xx.xx.xx.xx:8888/"

    ### gpg proxy 설정
    http-proxy="http://xx.xx.xx.xx:8888/"
    home 디렉토리 하위 .gnupg/gpg.conf
    아래와 같이 --keyserver-options 뒤에 넣습니다.
    RUN gpg --keyserver hkp://keyserver.ubuntu.com:80 --keyserver-options http-proxy=http://xx.xx.xx.xx:8888 --recv-keys \
