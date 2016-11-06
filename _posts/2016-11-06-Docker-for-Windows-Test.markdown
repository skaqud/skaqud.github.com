---
layout: post
title: Docker for Windows Test
date: 2016-11-06 21:49:00
tags:
- Tech
- 삽질
- Docker
---

(작성중)

이전에는 Docker Toolbox 라고 해서 가상환경을 띄워 그 안에 리눅스를 설치하고 Docker를 띄우는 방식으로 윈도우에서 Docker를 사용했었으나 금년 DockerCon 이후로 mac과 windows에서도 native하게(?) docker를 사용할 수 있게 되었다.

단 Docker for Windows의 경우 OS가 Windows 10 pro 64bit 이상이어야 함.


# 설치

https://docs.docker.com/docker-for-windows/

받은 후 실행하면 별 이상없이(?) 설치된다. cmd를 띄운 후 확인해 보니 버전은 1.12.1임

tomcat을 pull하고 실행해 보았다.

    $ docker pull tomcat
    $ docker run -d --name tomcat1 -p 8080:8080 tomcat

jdk도 깔지 않은 컴퓨터 상에서 아주 깔끔하게, 빠르게 tomcat 인스턴스가 실행된다. 개발용으로 쓰기엔 아주 좋을 듯 싶다.

설치 후 작업표시줄에 고래아이콘이 생김. 고래아이콘에서 오른쪽 마우스버튼을 누르면, disk를 공유하거나, 네트워크 설정을 할 수있는 것 같다.

## Kitematic 설치 및 실행

Open Kitematic 메뉴를 누르면 윈도우에서 사용할 수 있는 GUI를 다운로드 받게 되는데 Docker 설치 디렉토리 하위에 Kitematic이라고 생성한 뒤 받은 내용을 풀면 바로 해당 메뉴로 Kitematic을 띄울 수 있다.

Kitematic을 통해 CLI에서와는 다르게 Docker 명령을 알 필요 없이 컨테이너를 쉽게 검색하고 다운로드 받아서 빠르게 실행할 수 있다. 테스트삼아 tomcat과 redmine을 받아 보았는데, tomcat은 그렇다 쳐도 redmine은 조금은 복잡했던 설치과정이 아예 없어졌다. GUI를 통해 컨테이너를 시작하고 멈추고 설정을 조정하고 실행화면으로 바로 이동할 수 있다.

# 결론

- 앞으로 mac, window, linux 관계 없이 웬만한 서버형 솔루션 설치에는 docker가 필수적일 것 같다. 사실 그동안의 핑계는 "Linux에서밖에 안돼요" 가 가장 컸던 듯, windows 서버 환경에서 서비스 운영을 위한 부분이 어느 정도 검증된다면, 더 이상 핑계를 대고 docker로 바꾸는 걸 피하기 힘들 것 같다.
