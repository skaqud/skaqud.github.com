---
layout: post
title:  "Vagrant로 로컬에 가상 개발환경 준비하기"
date:   2016-04-11 16:15:49 +0700
categories: architecture
---
본 문서에서는 간략하게 Vagrant(와 Virtualbox) 를 이용하여 개발환경으로 사용할 가상 환경을 로컬에 생성하는 과정을 설명합니다. 아무래도 개인적으로 좀 더 거창하고 복잡한 Docker 보다는 vagrant가 더 접근하기 쉬웠던 것 같습니다.

# vagrant+Virtualbox 설치 #

- Vagrant - 최신 버전 1.8.1(https://www.vagrantup.com/downloads.html)
- Virtualbox - 최신 버전 5.0.20(https://www.virtualbox.org/wiki/Downloads)

다음과 같은 명령을 실행하여 로컬 환경에 linux를 설치

로컬 환경에 vagrant 디렉토리 생성, 디렉토리 이름은 상관 없으나(마음대로 생성 가능), 실행시키는 명령의 기본 디렉토리가 되므로, 앞으로 실행할 명령은 항상 다음 디렉토리 바로 아래에서 수행해야 합니다.

    mkdir ~/vagrant/centos

해당 디렉토리 안에서 다음과 같이 실행하면 Vagrantfile이 생성됨

    #vagrant init
    A `Vagrantfile` has been placed in this directory. You are now
    ready to `vagrant up` your first virtual environment! Please read
    the comments in the Vagrantfile as well as documentation on
    `vagrantup.com` for more information on using Vagrant.

다음과 같이 로컬에 box를 추가

    ubuntu의 경우
    #vagrant box add ubuntu/trusty64

    centos의 경우(선택하라고 나올 경우 virtualbox 버전 선택)
    #vagrant box add centos/7

다음과 같이 실행시 다음처럼 나오면 성공

    # vagrant box list
    centos/7        (virtualbox, 1603.01)
    ubuntu/trusty64 (virtualbox, 20160516.0.0)

이후 위에서 생성된 Vagrantfile의 다음 부분을 위에서 받은 box이름으로 수정합니다.

    (편집기로 Vagrantfile을 열어 중간의 다음 부분을)
    config.vm.box = "base"
    (다음과 같이 수정-설치 원하는 OS로)
    config.vm.box = "centos/7"

이후 다음과 같이 살행하여 VM을 실행시켜 줍니다.(시간이 다소 소요됨)

    ➜  temp vagrant up
    Bringing machine 'default' up with 'virtualbox' provider...
    ==> default: Importing base box 'centos/7'...
    ==> default: Matching MAC address for NAT networking...
    ==> default: Checking if box 'centos/7' is up to date...
    ==> default: Setting the name of the VM: temp_default_1463604899809_17904
    ==> default: Clearing any previously set network interfaces...
    ==> default: Preparing network interfaces based on configuration...
        default: Adapter 1: nat
    ==> default: Forwarding ports...
        default: 22 => 2222 (adapter 1)
    ==> default: Booting VM...
    ==> default: Waiting for machine to boot. This may take a few minutes...
        default: SSH address: 127.0.0.1:2222
        default: SSH username: vagrant
        default: SSH auth method: private key
        default: Warning: Connection timeout. Retrying...
        default: Warning: Remote connection disconnect. Retrying...
        default:
        default: Vagrant insecure key detected. Vagrant will automatically replace
        default: this with a newly generated keypair for better security.
        default:
        default: Inserting generated public key within guest...
        default: Removing insecure key from the guest if it's present...
        default: Key inserted! Disconnecting and reconnecting using new SSH key...
    ==> default: Machine booted and ready!
    ==> default: Checking for guest additions in VM...
        default: No guest additions were detected on the base box for this VM! Guest
        default: additions are required for forwarded ports, shared folders, host only
        default: networking, and more. If SSH fails on this machine, please install
        default: the guest additions and repackage the box to continue.
        default:
        default: This is not an error message; everything may continue to work properly,
        default: in which case you may ignore this message.
    ==> default: Rsyncing folder: /data/vagrant/temp/ => /home/vagrant/sync

이후 실행된 VM에 다음과 같이 접속할 수 있습니다.(반드시 아까 생성한 디렉토리에서 실행)

    ➜  temp vagrant ssh
    -bash: warning: setlocale: LC_CTYPE: cannot change locale (UTF-8): No such file or directory
    [vagrant@localhost ~]$

# 참고

Vagrant 소개

- http://taewan.kim/blog/2016/02/06/vagrant_intro/

- http://ppiazi.tistory.com/entry/Vagrant-VirtualBox-가상머신-관리-도구-소개

- http://bcho.tistory.com/806

Vagrant 설치 및 사용법 관련

- http://manseok.blogspot.kr/2014/09/vagrant.html

- http://rangken.github.io/blog/2015/vagrant-1/
