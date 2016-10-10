---
layout: post
title: Linux Container 용어
date: {}
tags:
  - Tech
published: true
---

Linux Container 관련 용어에 대한 설명(작성중)


# 용어

namespace - 독립되고, 가상화 된 공간을 제공하는 리눅스 기술, Docker나 LXC모두 namespace 기술을 기반으로 만들어 짐. hostname, filesystem, network, process, ipc등 알려진 컴퓨팅 자원을 모두 분할한다.

cgroup(Control Groups) - CPU,mem자원 사용률을 격리, 할당, 제한하는 Linux kernel의 기능 


chroot(change root) - 보안을 위해 새로운 가상 root 디렉토리를 생성하는 명령, 상위로 이동 불가능.


crouton - Chromium OS Universal Chroot Environment


# Docker, LXC 원리



크롬 에디터 - caret 추천

# 참고

https://blogs.rdoproject.org/7761/hands-on-linux-sandbox-with-namespaces-and-cgroups


