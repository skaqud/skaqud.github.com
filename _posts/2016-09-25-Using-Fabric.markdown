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

--> 배포 도구이기도 하지만, 다수의 서버들을 관리하는 관리자용 도구로도 사용할 수 있음 그에 대한 내용

## 설치방법

- 참고:[Fabric을 이용한 ssh streamlining](http://www.slideshare.net/ssuser0e3c90/posquit0-fabric-ssh-streamlining)

- 장 : 단순하고, 깔끔한 배포 도구, 개발자 입장에서 사용하기 쉬움

- 단 : GUI 없음, python 문법에 대한 Learning curve - GUI는 별도의 오픈소스가 있는 듯.

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

아래와 같이 간략히 테스트 해 볼 수 있음.

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

실행은 다음과 같이.

    $ fab hello

    $ fab host_info


Deploy 도구이지만, 의외로 다수의 서버들을 관리하는 데에도 효율적일 수 있을 것 같다.
