---
layout: post
title:  "자주 사용하는 명령들"
date:   2016-07-24 12:21:15 +0700
tags:
- reuse
- 삽질
---

* TOC
{:toc}

---

자주 사용하는 명령들의 모음(이 문서는 지속적으로 보완중입니다.)

# Linux

## 자주쓰는명령들

### tar명령

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

### find명령

    #find공백이 들어간 파일명 검색
    $ find ./ -name "* *"
    # 수정한지 20일 이상된 파일만 삭제 ( -exec rm {} \; )
    $ find . -mtime +20 -type f -ls -exec rm {} \;
    # 확장자가 .htm* .gif, .js, .css 인 것만 퍼미션을 644(rw-r--r--)로
    $ find . -name "*.htm*" -o -name "*.gif" -o -name "*.js" -o -name "*.css" -exec chmod 644 {} \;
    # 파일 크기가 300KB 이상( -size +300k )인 파일만
    $ find . -size +300k -ls
    #*..

### awk,xargs 명령

awk, xargs등의 명령은 ps, netstat 등과 연결하여 현재 떠있는 프로세스ID를 알아내거나, 그 프로세스들에 특정 작업을 하는 등 운영작업에 많이 사용됨.

    #특정 user의 process를 무조건 kill하는 명령
    $ ps -ef | grep user | awk '{print($1)}' | xargs kill -9
    #구분자를 :로 하여 첫번째 필드를 출력
    $ awk ‘FS=”:” {print $1}’ /etc/passwd

xargs는 표준 입력을 통해 명령 줄을 만들고 실행함. 너무 긴 명령행을 방지하기 위해 사용


## 참고가 될 Shell Script

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

## SSH 터널링

Linux 폐쇄망에서의 터널링 사례1

A-|-B-C-인터넷

위와 같은 상황에서 A는 직접적으로 인터넷을 사용할 수 없는데, A와 B가 SSH를 통해 연결할 수 있고, B에서 C의 특정 포트를 통해 인터넷을 사용할 수 있다고 하면(프록시) A는 B를 터널링하여 C를 통해 인터넷 환경에 접근할 수 있다. 주로 기업 환경에서 보안때문에 막혀 있는 경우 apt, yum등 패키지 업데이트나 패치 등을 위해 사용하기도 한다.

ssh USERID@B서버IP -N -L 28080:C서버IP:C서버포트

A서버에서 위와 같이 입력하면 B서버를 통해 C서버IP:C서버포트로 접근할 수 있는 터널이 생긴다. 로컬에서는 http_proxy 등의 값을 변경해주어 localhost의 28080 -> C서버IP:C서버포트 로 바로 접근할 수 있다.

Windows에서의 터널링 사례

(작성중)



## 상황별 해결방법

참고 - [생활코딩:SSH Key-비밀번호 없이 로그인](https://opentutorials.org/module/432/3742)

SSH로그인자동화(공개키를 이용한)

    #키 생성-rsa알고리즘으로 생성한다는 의미
    $ ssh-keygen -t rsa
    #타겟 서버로 복사
    $ scp ~/.ssh/id_rsa.pub vagrant@192.168.33.12:
    #혹은 다음과 같이 복사함.
    $ ssh-copy-id vagrant@192.168.33.12
    #해당 서버에 접속하여 허용 키 목록에 등록
    $ cat ~/id_rsa.pub >> ~/.ssh/authorized_keys
    #권한 설정
    $ chmod 700 ~/.ssh
    $ chmod 600 ~/.ssh/id_rsa
    $ chmod 644 ~/.ssh/id_rsa.pub  
    $ chmod 644 ~/.ssh/authorized_keys
    $ chmod 644 ~/.ssh/known_hosts
    #위의 작업을 해 준 계정에만 해당됨.(위에서는 vagrant)


OS 버전 확인

    #ubuntu
    #보안상의 이유로 바뀌어 있는 경우가 있음
    $ cat /etc/issue
    #그럴 경우 아래와 같이 수행
    $ cat /etc/*release*
    #혹은
    $ lsb_release -a

저장소(Repository)서버 변경(ubuntu) - 기본적으로 설정되어 있는 repository의 경우 대부분 서버가 외국이라, 속도가 상당히 드린 편임, 아래와 같이 국내서버로 변경한다.

    $ sudo vi /etc/apt/sources.list
    :%s/archive.ubuntu.com/ftp.daumkakao.com/g
    :%s/security.ubuntu.com/ftp.daumkakao.com/g
    :%s/extras.ubuntu.com/ftp.daumkakao.com/g
    $ apt-get update


시스템 시작시 자동으로 시작될 script

    /etc/rc.local 하위에 추가하면 됨

설치된 패키지 조회 및 제거

    (ubuntu)

    dpkg --get-selections | grep jdk

    RPM명령(CentOS,RHEL계열)

    #설치된 rpm 조회
    $ rpm -qa | grep chef
    #(위에서 이름을 확인 후)설치된 rpm 삭제
    $ rpm -e chef-12.4.1-1.el7.x86_64
    #rpm 설치
    $ rpm -ivh chef-12.5.1-1.el7.x86_64.rpm
    $ rpm -Uvh chef-12.5.1-1.el7.x86_64.rpm


## 운영시 유용한 명령들

    * 로그 크기 확인하기(일정 크기가 넘는 로그파일 확인)
    $ du -k | awk '{if (length($1) > 5) print}'
    * 특정 계정에 속한 프로세스 kill 하기
    $ ps -ef | grep 계정 | grep -v grep | awk '{print $2}' | xargs kill -9


# 그밖의 도구들

## Docker

빌드

    $ docker build -t ska/myapp:1.0 .
    $ docker build -t ska/java_web:1.0 .

실행

    $ docker run -d --name myapp -p 8080 ska/myapp:1.0
    $ docker run -i --name myapp -t ska/myapp:1.0 /bin/bash
    $ docker run -d --name java_web -p 8080 ska/java_web:1.0
    $ docker run -i --name java_web -t ska/java_web:1.0 /bin/bash

    #사용하지 않는 컨테이너 확인
    $ docker ps -a | grep Exited | awk '{print $1}'
    #사용하지 않는 컨테이너 전부 삭제
    $ docker rm $(docker ps -a | grep Exited | awk '{print $1}')
    #사용하지 않는 이미지 확인
    $ docker images | grep none | awk '{print $3}'
    #사용하지 않는 이미지 전부 삭제
    $ docker rmi $(docker images | grep none | awk '{print $3}')


## GIT

### 일반 명령

    # 변경 반영 및 commit, push
    $ git add --all(-A)
    $ git add ./*
    # gitignore에 있으나, 무시하고 등록하고 싶을 경우
    $ git add -f 파일명
    $ git commit -a -m "commit message"
    # push
    $ git push origin master
    # u옵션은 이후 push시 remote와 branch를 저장(이후로는 git push만 하면 됨)
    $ git push -u origin master

    # development 브랜치로 변경
    $ git checkout development
    # 브랜치 정보 보기
    $ git branch
    # 상태보기
    $ git status
    $ git log

    # 로컬에서 수정한 내용을 무시하고 싶을 때
    $ git checkout -- <파일명>

    # 전체를 무시하고 싶을 경우
    $ git fetch origin
    $ git reset --hard origin/master

참고 - [git - 간편 안내서](https://rogerdudler.github.io/git-guide/index.ko.html)

## Git 기타

### 바이너리 파일 표시

바이너리 파일의 경우 git에서 관리해줄 수 없는 포맷이라고 표시하는 게, git repository의 크기에도 좋고 관리에도 좋을 것 같음. 최상위 디렉토리에 .gitattributes 파일을 생성하고 다음과 같이 입력한다.

  \*.jpg binary

### gitignore 표시

서버로 push 하지 않거나, 로컬로 다운로드가 필요하지 않은 파일의 경우(서버와 로컬이 설정이 달라야 할 경우) 최상위 디렉토리에 .gitignore 파일을 만들고 제외할 파일을 입력한다.

예로 C# project의 gitignore는 다음과 같음.

[Example .gitignore file I use for C# projects](https://gist.github.com/kmorcinek/2710267)


### 설정

git 설정은 로컬의 사용자 홈 디렉토리에 .gitconfig 라는 이름으로 저장됨. 파일을 직접 수정해도 되나, 정확히 format을 모르므로, 아래와 같이 명령어 사용하는 게 편함.

    #글로벌 설정, email, username
    $ git config --global user.name "...."
    $ git config --global user.email ......

    # 프록시 설정
    $ git config http.proxy http://xx.xx.xx.xx:8888

    # https verify false - 프록시 사용시 https 인증서 문제가 있다면 사용
    $ git config http.sslVerify "false"


### git최신 버전을 설치하는 방법

대부분의 공식 repository(yum,apt-get)에는 비교적 이전버전에 들어가 있음

    # add-apt-repository 추가
    $ sudo apt-get install python-software-properties
    $ sudo apt-get install software-properties-common
    # 이후 아래와 같이 설치
    $ sudo su -
    $ add-apt-repository ppa:git-core/ppa
    # ---> 프록시 사용시에는 위의 명령을 반드시 sudo 없이 실행(sudo가 있을 경우 proxy는 못 탐)

    $ sudo apt-get update
    $ sudo apt-get install git

### git clone시 - 하위 디렉토리만 하기

기본적으로는 Repository 이름이 생기면서 하위로 다운로드 되므로, 원하는 디렉토리 아래 받고자 할 때 아래와 같이 수행한다.
(sparse checkout을 하라고 하는데, 특정 디렉토리만 받을 거 아니면 아래와 같이 하면 된다.)

    $ cd /
    $ git init srv
    $ cd srv
    $ git remote add -f origin https://github.com/blackmaz/saltstack_test.git
    $ git pull origin master

Sparse Checkout은 [여기](https://www.lesstif.com/pages/viewpage.action?pageId=20776761)를 참고

### git push 시 다음과 같은 오류가 발생하면

    $ git push origin master        
    Username for 'xxxx': ska
    Password for 'xxxx':
    Counting objects: 7, done.
    Compressing objects: 100% (6/6), done.
    Writing objects: 100% (7/7), 15.42 MiB | 0 bytes/s, done.
    Total 7 (delta 2), reused 0 (delta 0)
    error: RPC failed; HTTP 403 curl 22 The requested URL returned error: 403 Forbidden
    fatal: The remote end hung up unexpectedly
    fatal: The remote end hung up unexpectedly
    Everything up-to-date

로컬의 gitconfig 상에 proxy 설정 등이 있는지 확인, 없다면 buffersize, 그리고 서버 설정(gitlab의 nginx설정 등) 등의 문제일 수 있음.


## MySQL

    # DB생성
    CREATE SCHEMA ReviewDB DEFAULT CHARACTER SET utf8 ;
    # 계정 생성
    # MySQL은 접근시 모든호스트(%)로부터의 권한과 로컬(localhost)로부터의 권한을 별도로 줌
    create user 'gerrit'@'%' identified by 'gerrit123';
    grant all privileges on *.* to 'gerrit'@'%';
    create user 'gerrit'@'localhost' identified by 'gerrit123';
    grant all privileges on *.* to 'gerrit'@'localhost';
    flush privileges;

    Drop schema ReviewDB cascade;


## JDK

tar로 sun jdk 설치시 - apt-get 등으로는 sun-jdk를 바로 설치하기 좀 까다로움, 그래서 아래와 같이 tar를 특정위치에 풀고 update-alternatives 로 시스템에서 인식되도록 함.

    $ wget ......install_file_path/jdk-8u66-linux-x64.gz
    $ tar zxvf jdk-8u66-linux-x64.gz

    -ubuntu
    $ update-alternatives --install /usr/bin/javac javac /opt/jdk1.8.0_66/bin/javac 1
    $ update-alternatives --install /usr/bin/java java /opt/jdk1.8.0_66/bin/java 1
    $ update-alternatives --install /usr/bin/javaws javaws /opt/jdk1.8.0_66/bin/javaws 1


## Python

    바로 활용할 수 있는 단순 웹 서버(보통 python은 다 설치되어 있음)
    #run SimpleHTTPServer
    $ python -m SimpleHTTPServer 8080


## Screen 명령정리

예전 터미널 상에서의 screen 명령보다는 tmux를 쓰거나 terminator 등 멀티화면 지원하는 ssh client를 쓰는 게 나음.
tmux의 경우 사용법이 그나마 간단하고, 별도의 GUI를 가지고 있지 않아서 터미널상에서 바로 쓸 수 있는 멀티Screen도구일 것 같음.

    #설치
    $ apt-get install tmux
    #실행(실행시 아래 상태창이 생김)
    $ tmux
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
    $ export http_proxy="http://xx.xx.xx.xx:8888"
    $ export https_proxy="http://xx.xx.xx.xx:8888"

    #proxy setting(로그인쉘에 추가-bash의 경우)
    $ echo 'export http_proxy="http://xx.xx.xx.xx:8888"' >> ~/.bash_profile
    $ echo 'export https_proxy="https://xx.xx.xx.xx:8888"' >> ~/.bash_profile

    #apt proxy setting(ubuntu 패키지 관리자)
    $ echo 'Acquire::http::proxy "http://xx.xx.xx.xx:8888/";' >> /etc/apt/apt.conf
    $ echo 'Acquire::https::proxy "http://xx.xx.xx.xx:8888/";' >> /etc/apt/apt.conf

    #yum proxy setting(centos 패키지 관리자)
    $ echo 'proxy=http://xx.xx.xx.xx:8888' >> /etc/yum.conf

    #WINDOWS일 경우
    $ set HTTP_PROXY="http://xx.xx.xx.xx:8888"
    $ set HTTPS_PROXY="http://xx.xx.xx.xx:8888"

    #git(gitconfig) 설정
    $ git config http.proxy http://xx.xx.xx.xx:8888

    #docker proxy 설정
    참고 - http://nknu.net/running-docker-behind-a-proxy-on-ubuntu-14-04/

    /etc/defaults/docker
    파일 수정
    $ export http_proxy="http://xx.xx.xx.xx:8888/"

    ### gpg proxy 설정
    http-proxy="http://xx.xx.xx.xx:8888/"
    home 디렉토리 하위 .gnupg/gpg.conf
    아래와 같이 --keyserver-options 뒤에 넣습니다.
    RUN gpg --keyserver hkp://keyserver.ubuntu.com:80 --keyserver-options http-proxy=http://xx.xx.xx.xx:8888 --recv-keys \

    #android-sdk manager의 경우, 홈 디렉토리의 .android/androidtools.cfg 파일에 다음 내용 입력
    http.proxyLogin=USER@PASSWORD
    http.proxyPort=PORTNUMBER
    http.proxyHost=PROXYHTTP
