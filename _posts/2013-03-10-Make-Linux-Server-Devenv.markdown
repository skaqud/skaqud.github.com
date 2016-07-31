---
layout: post
title:  "Debian기반 Linux + tomcat + git개발환경 구성하기"
date:   2013-03-10 16:10:49 +0700
tags: 
- Tech
- 삽질
---

Debian기반 Linux + nginx + tomcat mysql + git개발환경 삽질 기록.

## 1. OS설치 및 환경설정 ##

### 1) crunchbang linux설치

---------------------------------------------------------------------

http://crunchbang.org/download/

(64bit 버전으로 다운로드)

vmplayer로 해당 iso지정하여 설치(설치하는 데 한글로 지정하여 별 문제 없었음)

    설치 이후 저장소 업데이트(콘솔을 열어 터미널에서 실행한다.)
    sudo apt-get update
    나눔고딕 폰트 설치 - 한글관련
    sudo apt-get install ttf-nanum
    openssh 서버설치
    sudo apt-get install openssh-server
    ssh데몬 재기동
    sudo /etc/init.d/ssh restart

ifconfig로 해당 OS의 IP를 확인하여 밖의 OS에서 ssh로 접근되는 지 확인
(ssh client로 확인)


### 2) jdk, tomcat 설치

---------------------------------------------------------------------

    openjdk설치
    sudo aptitude install openjdk-6-jdk
    sunjdk 설치
    sudo apt-get install sun-java6-jdk

(1)자동 설치 - 디렉토리가 자동으로 나뉘어 설치됨

    tomcat6 설치
    sudo apt-get install tomcat6 tomcat6-admin tomcat6-common tomcat6-user tomcat6-docs tomcat6-examples
    tomcat6 시작
    sudo /etc/init.d/tomcat6 start
    화면 확인
    http://192.168.118.129:8080
    설정파일 수정
    sudo vi /etc/tomcat6/server.xml

(2)수동 설치 - 톰캣 사이트에서 다운로드 후 설치, 홈 디렉토리 밑의 bin하위에 설치

(3)환경 설정
  GET 통해 한글 데이터를 받을때 서버에서 URL을 UTF-8로 인코딩하도록 설정

    # vi conf/server.xml
    <Connector port="8080" protocol="HTTP/1.1"
                   connectionTimeout="20000"
                   redirectPort="8443"
                   URIEncoding="UTF-8"/> ---> 추가


패스 설정(bash 로그인 스크립트 수정 --> 마지막 붙임.1 참조)

    CATALINA_HOME=/home/ska/bin/tomcat
    export CATALINA_HOME
    PATH=$PATH:$CATALINA_HOME/bin
    export PATH
    CLASSPATH=$CLASSPATH:$CATALINA_HOME/lib
    export CLASSPATH



### 3) nginx 설치 및 연동

---------------------------------------------------------------------

(1)파일 다운로드 및 설치

자동 설치

    apt-get update
    apt-get dist-upgrade
    apt-get install build-essential libpthread-stubs0 libpthread-stubs0-dev libgd2-xpm libgd2-xpm-dev libgeoip-dev libgeoip1 sysvinit openssl libssl-dev libpcre3-dev  libgcrypt11-dev

수동 설치(컴파일 설치)

    다운로드 받은 뒤 특정 위치에 풀고
    아래와 같이 미리 라이브러리를 받은 뒤
    sudo apt-get install libpcre3-dev
    sudo apt-get install libssl-dev
    환경설정 후 컴파일
    ./configure
    make install
    기본 설치경로 - /usr/local/nginx
    구동
    [ska@skaqud:/usr/local/nginx]$ sudo sbin/nginx
    종료
    종료는 # sbin/nginx -s stop 혹은 # sbin/nginx -s quit
    설정파일 등의 수정 후 재적용은 # sbin/nginx -s reload
    로그파일을 다시 열려면 # sbin/nginx -s reopen

확인

  http://192.168.118.129/

연동

  http://misoin.wordpress.com/2012/06/19/tomcat-nginx-%EC%97%B0%EB%8F%99%ED%95%98%EA%B8%B0/


정적 자원(html, js, css, image 등)은 기본적으로 nginx 를 이용하고,
동적 처리되는 부분에서는 tomcat를 사용하도록 하는 proxy 설정 방식이다.

    # vi conf/nginx.conf
    upstream backend {
        server  localhost:8080
    }

    server {
    ...(중략)
        location /servlet {
            proxy_pass      http://backend/servlet;
            index           index.jsp;
        }
    ...(후략)


연동확인

http://192.168.118.129/servlet/index.jsp 로 변경된 jsp파일 나오는 부분을 확인함.


### 4) svn/git 설치

---------------------------------------------------------------------

이클립스에서 git플러그인 설치
EGit플러그인은 Help -> Install New Software -> Work with에서 Indigo선택하시면 아래 Collaboration에서 Eclipse EGit를 설치



### 5) Mysql설치(DBMS설치)

---------------------------------------------------------------------

    sudo 매번 치기 귀찮으면 명령어 실행
    sudo su
    mysql 설치
    apt-get install mysql-server
    설치 및 구동확인
    netstat -tap|grep mysql

    원격에서도 접근 가능하도록 수정
    vi /etc/mysql/my.cnf
    디비를 재부팅한다.
    /etc/init.d/mysql restart

Mysql 접속 설정

    mysql -uroot -p
    권한 등록하기
    a. mysql 접속 후 mysql database 선택
    mysql> use mysql;
    b. user 테이블 살펴보기
    mysql> select host, user, password from user;
    root 의 host 값들은 localhost, 127.0.0.1 등으로 기본 등록되어 있지만, 외부접속을 나타내는 값이 없다.
    특정 아이피로 지정할 수도 있지만 여기선 % 기호로 어디서든 접속 가능하게 만든다.
    c. 권한 설정
    mysql> grant all privileges on *.* to 'root'@'192.168.118.1' identified by 'root의 패스워드';
    Query OK, 0 rows affected (0.03 sec)
    d. 등록확인하기
    mysql> select host, user, password from user;
    root 계정의 host 필드에 % 가 등록되었는지 확인한다.
    e. refresh
    mysql> flush privileges;
    Query OK, 0 rows affected (0.00 sec)

    계정 추가(egov)
    use mysql;
    insert into user(host,user,password) values('localhost','egov',password('egov'));
    insert into user(host,user,password) values('%','egov',password('egov'));
    flush privileges;

db 생성후 db에 계정연결

    grant all privileges on DB명.* to 계정명@localhost identified by '비밀번호' with grant option;
    flush privileges;


*** MySQL한글설정

    # mysql 또는 root 계정으로 작업을 권장합니다.
    vi /etc/my.cnf
    # client 부분밑에 추가
    [client]
    default-character-set = utf8
    # mysqld 부분밑에 추가
    [mysqld]
    init_connect = SET collation_connection = utf8_general_ci
    init_connect = SET NAMES utf8
    character-set-server = utf8
    collation-server = utf8_general_ci
    # mysqldump 부분밑에 추가
    [mysqldump]
    default-character-set = utf8
    # mysql 부분밑에 추가
    [mysql]
    default-character-set = utf8
    [저장후 나가기]
    # Mysql 재시작
    service mysqld restart 또는 /etc/init.d/mysqld restart
    # UTF8 확인하기
    # Mysql 접속
    mysql -u root -p [엔터]
    # 현재 캐릭터셋 확인
    show variables like 'c%'; [엔터]

    혹시 이전에 생성한 테이블이 있으면 캐릭터셋을 변경
    alter table 테이블명 convert to character set euckr;


### 6) hudson설치

---------------------------------------------------------------------

    hudson-ci.org에서 해당 hudson.war파일을 다운로드 받은 뒤
    톰캣을 재기동하고 아래와 같이 접근
    http://192.168.118.129:8080/hudson-3.0.1/




붙임1. .profile에 들어갈 내용들
---------------------------------------------------------------------

    ## aliases 'ls'
    alias ls="ls -hF --color"
    alias l="ls -l"
    #

    alias ..="cd ..;ls"   # cd 명령후 파일목록 리스트 바로 출력, ccd 참조
    alias be="vi ~/.bashrc;source ~/.bashrc"   # .bashrc 수정후 실행
    alias cls="clear"
    alias h="history"
    alias j='jobs -l'

    alias logs="tail -f /var/log/messages /var/log/*log"
