---
layout: post
title: PINE64 Cluster 구성하기(2)
date: 2017-02-23 22:01:00
tags:
- Life
- Tech
- 삽질
---

PINE64로 개인용 클러스터 테스트환경 구성하기(작성중)

PINE64 보드 4개를 연결하여, 개인적으로 테스트하기 위한 환경을 구성하는 과정을 정리함.

이전 글은 [여기로](https://skaqud.github.io/2016/10/08/PINE64/)


## 클러스터 환경 구성

이전에 참고 했고, 설치 이미지 등을 받았던 pine64.pro는 동작하지 않는 듯 하다. 이제는 아래 wiki를  이용.

http://wiki.pine64.org/index.php/Main_Page

클러스터를 구성하기 위해 동시에 전원을 꼽으면 같은 network에 연결되어 있을 경우 하나만 연결됨. 우연히 들은 게 있어서(mac이 겹치는 문제가 있다고 함..) mac변경을 위해 아래와 같이 조치. (근데 mac은 h/w에 박혀있는게 아닌가???)

### pine64 mac 주소 변경하는 방법

[no permanent MAC address? need it to give Pine64 a reserved IP.](http://forum.pine64.org/showthread.php?tid=743&pid=6939#pid6939)

뒤 상표 아래 나온 mac 주소를 다음 위치에 입력
vi /boot/uEnv.txt

ethaddr=xx:xx:xx:xx:xx:xx

변경 후 몇 번 재기동 한 이후로는 실제 변경된 mac으로 인식되며, 같은 네트워크 상에 물려 있더라도 동시에 접근 가능함.

### 클러스터 환경 설정

ip는 편의상 192.168.1.11~4번으로 설정함, 11번이 2GB짜리 master

static ip 설정은 다음과 같이 함

다음 내용을 /etc/network/interfaces 안에 설정, dns설정이 없으면 dns해석을 못함.

    auto eth0
    iface eth0 inet static
    address 192.168.1.1x
    gateway 192.168.1.1
    netmask 255.255.255.0
    dns-nameservers 8.8.8.8 8.8.4.4

    네트워킹 재시작
    /etc/init.d/networking restart

master는 2GB 이기도 하고, desktop환경도 한 번 설치해 보고 싶기도 하여 다음과 같이 설치

위키를 보고 다음과 같이 실행

    sudo /usr/local/sbin/install_mate_desktop.sh




### 클러스터 서버 정리하기

검색 필요, 굳이 케이스까진 필요 없을 것 같은데, 그냥 놔두면 등짝감이라. 뭔가 4개의 보드,전원,네트워크 선을 정리할 방법이 필요

인터넷 상에는 레고로 케이스 제작하거나, 아크릴로 자작하기도 하고, 육각나사를 사서 보드 네 귀퉁이를 연결하는 방법도 있는 것 같음.

http://sylvi.tistory.com/10

위처럼 볼트형 지지대(10mm) + 너트형 지지대(10mm) + 볼트형 지지대(25mm)를 연결하면 될 듯




# 참고

[PINE64 wiki](http://wiki.pine64.org/index.php/Main_Page)
