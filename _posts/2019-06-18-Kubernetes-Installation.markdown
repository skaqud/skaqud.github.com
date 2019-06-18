---
layout: post
title: Kubernetes Installation
date: 2019-06-18 22:15:11
tags:
- Tech
- Docker
- Kubernetes
---

Kubernetes Installation 방법(작성중)

# Kubernetes

Goole에서 만든 컨테이너 오케스트레이션을 위한 플랫폼. 사실상 표준이 되어가고 있는 듯 함. 여러개의 노드(서버) 상에서 컨테이너를 자동으로 생성,관리해준다. 구글에서 내부적으로 오랜기간 사용해오던 환경을 오픈소스화 하였다고 함.
기본적으로 master-nodes 구성으로 master는 관리 및 제어작업만 해 줌, multi master도 가능하다 여기서는 sinle master,multi node로 설치해 볼 예정


설치 환경 ubuntu 18.04, 2core 2GB의 VM 4대 -  KVM을 통해 가상환경을 실행하여 테스트

# Pre-Installation

KVM 설치 및 환경설정작업


# Installation

각각의 노드 별로 작업해 주어야 함.

0. docker install

1. Kubernetes Install


- repository구성

- selinux off / firewalld stop


- kernel parameters set - 네트워크 브리지 활성화

- swap off - 쿠버네티스는 스왑을 반드시 꺼야 함


- kubernetes start

- Master 구성(master에서만 실행)

- node에서 master로 토큰을 통해 연결

- vxLAN 설치(weavenet)

# 추가작업

- 모니터링 도구 설치

- 대시보드 설치

