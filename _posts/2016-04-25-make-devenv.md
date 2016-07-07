---
layout: post
title:  "WEB,WAS,DB 설치 및 웹 어플리케이션 구성을 위해 연동하기"
date:   2016-04-25 23:01:49 +0700
categories: architecture
---

본 문서에서는 준비된 가상 개발환경에 WEB,WAS,DB를 설치하고 Sample Application을 deploy하여 확인하는 과정에 대해 설명합니다.

# 개요

다음과 같은 환경에서 로컬에 생성된 linux(CentOS) 가상환경(VM)을 기반으로 WEB-WAS-DB를 구성하고 Sample application 을 deploy 하여 테스트합니다.

세부 테스트환경

- OS : CentOS 7(vagrant box:centos/7)
- WEB : nginx(yum repository 버전)
- WAS : apache-tomcat 7.x,(openjdk 1.8)
- DB - mariadb
- WebApp : spring-petclinic(https://github.com/spring-projects/spring-petclinic)



# 실행하기

우선은 vagrant + virtualbox로 linux vm을 로컬에서 시작

해당 vm에서 다음과 같이 패키지 관리자로 최신 OS 로 업데이트

	yum update

이상 없이 업데이트 될 경우 기본적인 준비가 된 것으로 봅니다.

### 0) 계정 생성

	#계정 생성
	adduser testu

	#비번 설정
	passwd testu

이후 WAS, DB의 기본적인 작업은 위의 계정으로 실행합니다.

### 1) DB 설치

참고 : http://firstboos.tistory.com/entry/CentOS-7-에서-mariadb-설치

위 블로그 내용을 참고해서 기본 패키지관리자(yum)를 통해 설치합니다.

	yum install mariadb-server

블로그 내용대로 characterset 설정(utf-8)을 한 뒤 서비스를 시작

	service mariadb start
	#혹은 다음과 같이 실행
	#systemctl mariadb start

root 비번을 적당히 설정한 뒤 DB상태를 확인합니다.(root 비번을 까먹지 않도록 주의합니다.)

위에서 생성한 계정(testu)로도 로그인 되는지 확인합니다.


### 2) WAS 설치

우선 tomcat에서 사용할 jdk를 설치합니다.

	yum search openjdk | grep jdk

목록을 확인하여 java 1.7 jdk를 설치합니다.

	yum install java-1.7.0-openjdk.x86_64

다음과 같이 설치를 확인합니다.

	[root@localhost ~]# java -version
	java version "1.7.0_101"
	OpenJDK Runtime Environment (rhel-2.6.6.1.el7_2-x86_64 u101-b00)
	OpenJDK 64-Bit Server VM (build 24.95-b01, mixed mode)

위에서 생성한 계정으로 로그인한 후 tomcat을 다운로드 합니다. 다운로드 주소는 이곳에서 확인, tomcat의 경우 binary를 다운로드 받은 뒤 특정 디렉토리에 설치합니다. webapps 들이 deploy되어야 하기에 일반적으로 관리될 수 있는 디렉토리에 위치시키는 게 좋을 것 같습니다.

	#계정 변경(testu)
	su - testu

	mkdir app
	cd app
	wget http://apache.mirror.cdnetworks.com/tomcat/tomcat-8/v8.0.35/bin/apache-tomcat-8.0.35.tar.gz

압축을 해제한 뒤 tomcat을 시작하여 정상적으로 기동되는 지 확인합니다.

    tar zxvf apache-tomcat-8.0.35.tar.gz
    cd apache-tomcat-8.0.35/bin
    ./startup.sh
    [testu@localhost bin]$ ./startup.sh
	Using CATALINA_BASE:   /home/testu/app/apache-tomcat-8.0.35
	Using CATALINA_HOME:   /home/testu/app/apache-tomcat-8.0.35
	Using CATALINA_TMPDIR: /home/testu/app/apache-tomcat-8.0.35/temp
	Using JRE_HOME:        /
	Using CLASSPATH:       /home/testu/app/apache-tomcat-8.0.35/bin/bootstrap.jar:/home/testu/app/apache-tomcat-8.0.35/bin/tomcat-juli.jar
	Tomcat started.

curl명령 혹은 브라우저를 통해 실행을 확인합니다.


### 3) WEB 설치

아래와 같이 repository를 추가하여 nginx 저장소를 등록한 뒤

	vi /etc/yum.repos.d/nginx.repo

	[nginx]
	name=nginx repo
	baseurl=http://nginx.org/packages/centos/7/$basearch/
	gpgcheck=0
	enabled=1

nginx를 설치합니다.

	yum install nginx

이후 서비스를 시작하고,

	systemctl start nginx

	[root@localhost ~]# ps -ef | grep nginx
	root      2178     1  0 10:33 ?        00:00:00 nginx: master process /usr/sbin/nginx -c /etcnginx/nginx.conf
	nginx     2180  2178  0 10:33 ?        00:00:00 nginx: worker process

curl명령 혹은 브라우저를 통해 실행을 확인합니다.



### 4) WAR 준비 및 테스트

이제까지 진행된 내용들은 vm에서 작업되었으나, 다음 내용은 어느 환경에서 수행하던지 상관이 없습니다. 최종 테스트시에는 이 과정을 통해 생성된 war파일만을 사용합니다. 이클립스 툴을 사용해야 하므로 보통은 VM말고 host pc에서 수행합니다.

다운로드 페이지(https://eclipse.org/downloads/)에서

Java EE Developers verison을 다운로드 한 뒤 원하는 곳에 압축을 풀고 실행합니다.

이클립스가 정상적으로 실행되는지를 확인한 뒤, spring-petclinic프로젝트(https://github.com/spring-projects/spring-petclinic)를 다운로드 받아 import 합니다.

import 방법에는 여러가지가 있습니다.

- 이클립스 상에서 git플러그인(egit)을 사용하여 import

- 다른 git client를 이용해서 로컬에 다운로드 받은 뒤 해당 repository를 import 하는 방법

- 해당 사이트에서 zip을 다운로드 받아 이클립스에 import 하는 방법

다음과 같이 이클립스 상에서 git 플러그인을 통해 직접 export 하는 방법으로 import 합니다.

Package Explorer에서 오른쪽 마우스 클릭 > import 선택 후 Projects from GIT 선택

-> Clone URI 선택 후 해당 project의 git 정보를 입력하여 import

-> git clone 후 프로젝트 import 시 import as general project 로 선택하여 import 수행


해당 프로젝트가 로컬에 import되면, 다음과 같은 사항들을 확인합니다.

- 해당 프로젝트 명에서 오른쪽 마우스 클릭 > Properties에서 Project facet의 정보 중 Java와 Dynamic web module이 선택되어 있는지 확인

- Maven 프로젝트인지 확인하여 안되어 있을 경우 Configure > Convert to Maven Project를 선택하여 Maven Project로 변경

이상이 완료되면, 한 번 실행하여 실행여부를 확인합니다. "Run as" > "Maven Clean" 후 "Maven Install" 을 선택하여 패키징을 수행합니다. 초기 수행시 Maven depedency에 의해 관련된 라이브러리를 다운로드 하는 데 시간이 좀 걸리며, 이후 target 디렉토리에 petclinic.war 가 생기면 성공적으로 완료된 것입니다.

단 원래 버전은 HSQL 기반이므로, data-access.properties 파일의 DB설정부분을 MySQL 용으로 변경하고 pom.xml 파일의 다음 부분의 압축을 푼 뒤 다시 패키징을 수행하여 정상적으로 Mariadb로 연결될 수 있는 바이너리를 생성합니다.

        <!-- 다음 부분의 주석 해제(원래 주석처리 되어 있음) -->
        <!-- For MySql only -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>${mysql-driver.version}</version>
        </dependency>

Maven Install 시 테스트를 skip하기 위해서 pom.xml 파일의 다음부분에 skipTest태그를 넣습니다.(그렇지 않을 경우 로컬에서 테스트Case 오류가 발생)

            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
                <version>2.13</version>
                <configuration>
                	<!-- 아래 내용을 추가  -->
                    <skipTests>true</skipTests>
                    <includes>
                        <include>**/*Tests.java</include>
                    </includes>
                </configuration>
            </plugin>


### 5) Deploy 및 통합테스트

위에서 준비한 petclinic.war를 VM으로 옮깁니다. 2)에서 설치한 apache tomcat의 하위 디렉토리 중 webapps 하위 디렉토리에 가져다 놓은 뒤 tomcat을 재기동하면 자동으로 web application이 압축이 풀리며 시작됩니다.

웹 어플리케이션이 성공적으로 시작되었으면 브라우저를 통해 확인합니다.

이후 web, db와 연동하기 위해 다음과 같이 수행합니다.

WEB-WAS 연동은 nginx 설정을 수정하여 사용자가 nginx 의 80포트를 호출하면 WAS의 서비스포트로 연결해주는 역할이며, 이는 nginx를 reverse proxy로 사용하는 방식입니다

다음 nginx 설정을 열어

	vi /etc/nginx/conf.d/default.conf

	#add thid configuration
    location /petclinic/ {
        proxy_pass http://localhost:8080/petclinic/;
    }

위의 내용을 맨 server 영역 안쪽부분에 추가합니다. 이후로는 밖으로 8080이 열려 있지 않아도 해당 서비스(petclinic)에 대하여 80포트를 통해 서비스할 수 있습니다.(서비스 재기동 필요)

DB의 경우 WAS에 설정된 DB 정보를 위의 3) 에서 설치된 DB 정보로 변경해 주어야 합니다.

설정파일을 찾기 위해 해당 어플리케이션이 설치된 위치로 갑니다.

4번 과정을 수행해서 생성된 petclinic.war를 2번에서 설치한 ~/app/tomcat경로/webapps 하위에 복사합니다.
현재 tomcat이 구동중이라면 자동으로 war를 인식하여 압축을 풀면서 자동으로 어플리케이션을 올립니다.
DB 정보를 수정하기 위해서 압축이 풀린 디렉토리를 탐색하여 정보를 수정합니다.

다음 경로 하위의
/home/testu/app/apache-tomcat-8.0.35/webapps/petclinic/WEB-INF/classes/spring
data-access.properties 파일의 내용을 열면 HSQL과 MySQL 부분에 대한 설정이 보입니다. 현재 열려 있는 HSQL부분을 주석처리 한 뒤 MySQL부분의 주석을 해제한뒤 아래 내용처럼 입력해줍니다.

    # MySQL Settings
	jdbc.driverClassName=com.mysql.jdbc.Driver
	jdbc.url=jdbc:mysql://localhost:3306/petclinic?useUnicode=true&characterEncoding=UTF-8
	jdbc.username=petclinic
	jdbc.password=petclinic

    # Property that determines which database to use with an AbstractJpaVendorAdapter
	jpa.database=MYSQL							

끝으로 위에서 입력한 정보대로 DB, 계정을 생성해줍니다.

	CREATE DATABASE petclinic DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;
	create user 'petclinic'@'%' identified by 'petclinic';
	create user 'petclinic'@'localhost' identified by 'petclinic';
	grant all privileges on petclinic.* to 'petclinic'@'%';
	grant all privileges on petclinic.* to 'petclinic'@'localhost';

접속을 테스트 해 본 뒤 기능이 제대로 동작하는 지 확인합니다.

수정 후 tomcat을 재시작하여 이상없이 데이터가 나오는지 확인합니다.

최종적으로 80 포트를 통해 서비스를 확인하면 web - was - db 연동이 제대로 된 것이라고 할 수 있습니다.
