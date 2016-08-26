---
layout: post
title:  "Docker 컨테이너를 빌드하기"
date:   2016-08-21 18:30:00
tags:
- 삽질
- Tech
- Docker
---

로컬에서 docker를 통해 jekyll을 빌드하고 실행하는 방법.(작성중입니다.)

# 빌드 테스트

다음과 같은 docker 파일을 생성

    FROM ubuntu:14.04
    MAINTAINER ska <ska202@gmail.com>

    RUN apt-get update
    RUN apt-get -y upgrade

    RUN apt-get install -y curl && \
    gpg --keyserver hkp://keys.gnupg.net --recv-keys D39DC0E3 && \
    \curl -sSL https://get.rvm.io | bash -s stable && \
    /etc/profile.d/rvm.sh && \
    rvm install ruby-2.2.0

    RUN apt-get -y install nodejs
    RUN gem install jekyll

빌드는 다음과 같이 수행합니다.

    docker build -t ska/jekyll:0.1 .
