---
layout: post
title: AWS in action 리뷰
date: 2017-05-29 24:12:00
tags:
- Tech
- Review
---

한빛출판사 나는 리뷰어다 이벤트, Amazon Web Service in action 서적의 리뷰

AWS에 대해 사용해보지 않은 사람들이 Tutorial로 사용할 수 있을 정도로 친절하다.



# 소감

-

AWS 에 대한 소개 및 자랑?

Cloudformation을 통한 인프라 자동 생성, immutable infrastructure의 개념 하에, 서버 설치 후 상태의 변경이 없도록 하는 도구.

AWS는 Xen 기반 오픈소스 하이퍼바이저를 사용, HVM, 인텔 Vt-x 플랫폼, 가상 리눅스 서버로 3.8+ 이상의 커널을 사용하면 최상의 성능이 날 수 있음.

가상서버 사용하기

인스턴스 생성 시에 Tags는 기술적으로는 크게 의미가 없다. 인스턴스가 많아진다면 체계적으로 이름을 넣어야 함.

인스턴스 Launch후 키를 생성하는데, 잊어버렸다고 해도 다시 생성 가능하다. 윈도우 환경에서는 pem을 puttygen등을 통해 ppk로 변경해야 putty를 통해 접속할 수 있다.
