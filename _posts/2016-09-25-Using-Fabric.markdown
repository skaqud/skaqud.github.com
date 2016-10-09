---
layout: post
title:  "Fabric 사용법"
date:   2016-09-25 23:12:00
tags:
- Tech
- Fabric
- devops
---

배포 도구인 Fabric 사용법

개인적으로 Capistrano는 너무 어려웠음, 대안으로 찾은게 그나마 더 친근한(?) Python 기반의 fabric을 선택함.

참고 - [How To Use Fabric To Automate Administration Tasks And Deployments](https://www.digitalocean.com/community/tutorials/how-to-use-fabric-to-automate-administration-tasks-and-deployments)

fabric 설치

- 참고:[Fabric을 이용한 ssh streamlining](http://www.slideshare.net/ssuser0e3c90/posquit0-fabric-ssh-streamlining)

장 : 단순하고, 깔끔한 배포 도구, 개발자 입장에서 사용하기 쉬움

단 : GUI 없음, python 문법에 대한 Learning curve - GUI는 별도로 만들고 있는 것 같긴 함.

--------------------------------------------------------
pip를 통한 설치

    $ curl -O https://raw.github.com/pypa/pip/master/contrib/get-pip.py
    $ curl -O https://bootstrap.pypa.io/get-pip.py
    $ [sudo] python get-pip.py
    $ gcc install
    $ pip install pycrypto fabric

혹은 아래와 같이 설치(더 간편)

    $ sudo apt-get install fabric
--------------------------------------------------------

적절한 디렉토리에 아래와 같이 fabfile.py 생성
env.hosts = [
        '192.168.1.9',
        '192.168.1.10'
]

def hello():
    print("Hello world!")

def host_info():
    local('uname -a')
    run('uname -a')

def host_run():
    # Create a directory (i.e. folder)
    run("mkdir trunk")
    run("rmdir trunk")

    # Uptime
    run("uptime")

    # Hostname
    run("hostname")

    # Capture the output of "ls" command
    result = run("ls -l")

    # Check if command failed
    result.failed

실행

fab hello

fab host_info
