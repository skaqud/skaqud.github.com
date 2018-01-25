---
layout: post
title: Python 개발관련 참고
date: 2017-09-24 11:04:00
tags:
- Tech
- Development
- Reference
---

현재 진행중인 Python api proxy 개발관련 참고사항

# 개발참고

minimal http server
- https://gist.github.com/bradmontgomery/2219997

logging 추가하는 부분
- http://gyus.me/?p=418

python hmac method(api인증)
- https://www.jokecamp.com/blog/examples-of-creating-base64-hashes-using-hmac-sha256-in-different-languages/#python

request 모듈에 대한 설명
- http://dgkim5360.tistory.com/entry/python-requests

python httprequesthandler
- https://mafayyaz.wordpress.com/2013/02/08/writing-simple-http-server-in-python-with-rest-and-json/


# 트러블슈팅

intelliJ에서 외부 디렉토리가 있는데, 참조하지 못할 경우

- pip등으로 다시 설치해 본다.
  - virtualenv로 라이브러리 디펜던시 문제 없이 개발이나 테스트 가능(사용해 보기)
- IntelliJ에서 해당 프로젝트의 디렉토리를 source root로 변경한다.
  - Project View(?)에서 해당 프로젝트, 디렉토리에서 오른쪽 마우스 클릭 > Mark Directory As 메뉴에서 souce root로 지정

윈도우에서는 설치 후 파이썬 설치 디렉토리와 Script 디렉토리까지 path에 넣어야 함



# python unittest

참고 - [python 테스트 시작하기](https://www.slideshare.net/hosunglee948/python-52222334)
참고2 - [예제로 배우는 Python프로그래밍](http://pythonstudy.xyz/python/article/21-유닛-테스트)

tests디렉토리 하위에 테스트코드가 있을 경우 cli에서 실행은 다음과 같이 

    python -m tests.testName


# Python 프로젝트 구조관련

참고 - [python - 전형적인 테스트 디렉토리 구조로 unittest 실행하기](http://code.i-harness.com/ko/q/1cf1d6)
참고2 - [Python Project Howto](http://infinitemonkeycorps.net/docs/pph/)




