---
layout: post
title: Node.js 설치
date: 2017-08-09 00:48:00
tags:
- Tech
- 삽질
---

Node 설치, 실행 방법


# 설치 및 환경구성

설치

참고 : https://nodejs.org/en/download/package-manager/#debian-and-ubuntu-based-linux-distributions

  curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
  sudo apt-get install -y nodejs


proxy 설정(필요시)

  npm config set proxy http://xxx.xxx.xxx.xxx:xxxx
  npm config set https-proxy http://xxx.xxx.xxx.xxx:xxxx
  npm config set strict-ssl false


  # yo 설치
  npm install -g yo
  # express 설치
  npm install -g express
  npm install -g express-generator

mongodb 설치 - 테스트를 위해 필요

https://www.digitalocean.com/community/tutorials/how-to-install-mongodb-on-ubuntu-14-04
