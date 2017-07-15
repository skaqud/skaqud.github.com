---
layout: post
title: 완벽한 IT인프라 구축을 위한 Docker 소감
date: 2017-06-28 22:03:00
tags:
- Tech
- Review
---

완벽한 IT 인프라 구축을 위한 Docker - 정보문화사 - 소감

인프라 엔지니어 입장에서 Docker에 대한 접근 방법을 정리.
앞쪽에서는 Docker 이전의 인프라 기술들(네트워크, 서버, Linux, 가상화 기술들)에 관해 체계적으로 기술적 배경에서부터 접근하나 깊게는 들어가지 않음. 뒷부분은 Docker를 활용한 실습이 대부분. Docker를 처음 접하는 사람들에게 적합할 듯 하다.
저자가 일본사람이라 그런건지, 삽화가 많고 책의 번역 상태가 서양쪽 저자 책 보다는 이해하기 쉬운 느낌, 내용이 쉽게, 체계적으로 잘 되어 있으며, 언급되는 내용들이 우리 프로젝트 환경이랑 비슷한 느낌도 받을 수 있음(예를 들어 작은, 단기 프로젝트의 경우 인프라 관리를 신입이나, 아마추어 개발자가 떠안을 수 있다는 부분 등)

다음은 일부 내용들임.


- 리눅스의 파일 시스템 - VFS, 모든 디바이스를 파일로 취급
- 리눅스의 모든 디렉토리는 FHS(Filesystem Hierachy Standard)로 표준화

인프라 구성관리 도구의 분류

- Bootstrapping Tool: OS설치, 설정등을 자동화하는 도구
- Configuration Management Tool : 설치, 설정, 보안 관리도구, Chef,Puppet,Ansible
- Ochestration Tool:Serf,Capistrano,Kubernetes

Docker의 동작구조

- 컨테이너를 분리하여 관리하는 구조 - namespace : PID,Network, UID, MOUNT, UTS, IPC
- 리소스 관리 구조 - cgroup : thread를 그룹화하여 이를 관리하는 기능
- 네트워크 구성 - NAT, NAPT
