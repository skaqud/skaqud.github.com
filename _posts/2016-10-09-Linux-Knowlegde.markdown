---
layout: post
title: Linux Container 용어
date: 2016-10-09 23:27:00
tags:
  - Tech
  - Docker
---

Linux Container 관련 용어에 대한 설명(작성중)


# 용어

namespace - 독립되고, 가상화 된 공간을 제공하는 리눅스 기술, Docker나 LXC모두 namespace 기술을 기반으로 만들어 짐. hostname, filesystem, network, process, ipc등 알려진 컴퓨팅 자원을 모두 분할한다.

참고 - [Linux namespace](http://bluese05.tistory.com/11)

cgroup(Control Groups) - CPU,memory 등 자원 사용률을 격리, 할당, 제한하는 Linux kernel의 기능

참고 - [linux - cgroups (control groups)](http://linuxism.tistory.com/1601)

chroot(change root) - 보안을 위해 새로운 가상 root 디렉토리를 생성하는 명령, 상위로 이동 불가능.

기타

crouton - Chromium OS Universal Chroot Environment, 크롬북의 Linux 별도 설치 환경

크롬OS에서 Linux를 설치하기 위해 사용하는

# Docker, LXC 원리

Docker 도 LXC(LinuX Containers)기술의 일종, 컨테이너형 가상화 기술은 기존의 가상화 기술보다 가벼워지고, 이식성이 뛰어남, 컨테이너 기술은 커널 공간에 있는 namespaces, cgroups, SELinux 등의 기술을 이용해 Containers 공간을 여러개로 나누어 사용자 프로세스에서 보이는 리소스를 제한하는 게 기본적인 원리임. 여러 사용자의 프로세스를 정리하여 분리한 사용자 공간이 바로 컨테이너 임.

그밖의 컨테이너 기술들

- OpenVZ
- libvirt
- Docker
- systemd

[OpenVZ란?](https://ko.wikipedia.org/wiki/OpenVZ)

# 참고

https://blogs.rdoproject.org/7761/hands-on-linux-sandbox-with-namespaces-and-cgroups

[LXC 와 Docker 그리고 Linux 컨테이너 기술들 살펴보기](http://opennaru.tistory.com/105)
