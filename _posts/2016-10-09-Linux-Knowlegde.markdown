---
layout: post
title: Linux Container 용어
date: 2016-10-09 23:27:00
tags:
  - Tech
  - Docker
---

Linux Container 관련 용어에 대한 설명

docker가 뭐하는 건지는 알겠는데, 어떤 원리로 돌아가는지 용어 자체를 모르겠어서 정리해 봄.


# 용어설명

`namespace` - 독립되고, 가상화 된 공간을 제공하는 리눅스 기술, Docker나 LXC모두 namespace 기술을 기반으로 만들어 짐. hostname, filesystem, network, process, ipc등 알려진 컴퓨팅 자원을 모두 분할한다.

참고 - [Linux namespace](http://bluese05.tistory.com/11)

`cgroup`(Control Groups) - CPU,memory 등 자원 사용률을 격리, 할당, 제한하는 Linux kernel의 기능

참고 - [linux - cgroups (control groups)](http://linuxism.tistory.com/1601)

chroot(change root) - 보안을 위해 새로운 가상 root 디렉토리를 생성하는 명령, 상위로 이동 불가능. 이 기능을 이용해서 ftp나 타 서비스에 활용하여 보안을 강화하는 데 사용하기도 함.


# Docker의 원리

Docker 도 Linux 컨테이너 기술의 일종, 컨테이너형 가상화 기술은 기존의 가상화 기술보다 가벼워지고, 이식성이 뛰어남, 컨테이너 기술은 커널 공간에 있는 namespaces, cgroups, SELinux 등의 기술을 이용해 Containers 공간을 여러개로 나누어 사용자 프로세스에서 보이는 리소스를 제한하는 게 기본적인 원리임. 여러 사용자의 프로세스를 정리하여 분리한 사용자 공간이 바로 컨테이너 임.

장점 - fast(vm보다), agile, Flexible, lightwhight, 효율적?

Docker 0.9 version 이후로 docker의 execution driver가 Docker 엔진으로부터 분리되어 기본인 libcontainer뿐 아니라, OpenVZ, LXC, systemd-nspawn, libvirt 등을 선택하여 사용할 수 있음. libcontainer 는 docker에서 개발한 자체 실행 드라이버이며, GO언어로 개발되어 있음.

-e 플래그로 다른 컨테이너 엔진을 사용 가능

docker -d -e lxc

Docker 이미지는 유니온 파일 시스템(AUFS)을 이용해 변경된 부분만 저장하고 있음.


# 그밖의 컨테이너 기술들

- OpenVZ : [OpenVZ란?](https://ko.wikipedia.org/wiki/OpenVZ)

- libvirt

- Docker(libcontainer) : Docker에서 개발된 container executor

- [systemd](https://ko.wikipedia.org/wiki/Systemd) : Redhat 7부터 적용된 리눅스 시스템 관리자

- [systemd-nspawn](https://www.freedesktop.org/software/systemd/man/systemd-nspawn.html) : systemd-nspawn may be used to run a command or OS in a light-weight namespace container. In many ways it is similar to chroot(1), but more powerful since it fully virtualizes the file system hierarchy, as well as the process tree, the various IPC subsystems and the host and domain name.

- [LXC](https://ko.wikipedia.org/wiki/LXC)

# 참고

[LXC 와 Docker 그리고 Linux 컨테이너 기술들 살펴보기](http://opennaru.tistory.com/105)

# 기타

[KVM and docker LXC Benchmarking with OpenStack](http://www.slideshare.net/BodenRussell/kvm-and-docker-lxc-benchmarking-with-openstack)

[Hands on Linux sandbox with namespaces and cgroups](https://blogs.rdoproject.org/7761/hands-on-linux-sandbox-with-namespaces-and-cgroups)

crouton - Chromium OS Universal Chroot Environment, 크롬북의 Linux 별도 설치 환경
