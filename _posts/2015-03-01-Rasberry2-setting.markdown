---
layout: post
title:  "라즈베리파이2 설치 및 사용기"
date:   2015-03-01 22:56:20
tags:
- life
- tech
- 삽질
---

노리고 있던 라즈베리파이2를 구입하여 집 안에서 아기에게 틀어주기위한 동영상NAS로 세팅하기까지의 과정

# 개요

집 안에서 NAS 역할을 할 서버에 대해 궁리하다가, 맥미니, 베어본PC, 스틱PC 등 여러가지 들을 궁리해봤으나, 가격, 성능, 불안정함(?) 등으로 선뜻 시도를 해 보지 못하고 있다가, 라즈베리파이2가 마침 나와서, 사서 안되면 딴거라도 해 보면 되지 하고 지름.

사실 GPIO, 파이썬, 아두이노 등과 연결하여 좀 더 창의적이고, 유익한 쓰임(?)으로 사용할 수 있을 것 같으나, 그렇게 크리에이티브하고, 부지런한 인간은 아니라서 세팅하면서 나왔던 내용을 적음.

# 초기 설치 및 설정

OS image 다운로드 - rasbian 이라고 하며 debian의 변형판이다.

https://www.raspberrypi.org/downloads/raspbian/

SD카드(4~8GB)를 구해 위의 이미지를 SD카드에 쓴다. 윈도우에서는 [Win32DiskImager](https://sourceforge.net/projects/win32diskimager/) 를 이용하여 설치한다.

SD카드를 넣고 라즈베리파이에 전원을 넣어 부팅한다. 보드에서 불이 반짝반짝 하며 부팅된다. 처음에는 모니터가 연결되어 있어야 함(IP를 모르므로..)

외장하드가 있을 경우 부팅시 인식하도록 다음과 같이 /etc/fstab에 추가

    raspberrypi:~$ cat /etc/fstab
    proc            /proc           proc    defaults          0       0
    /dev/mmcblk0p1  /boot           vfat    defaults          0       2
    /dev/mmcblk0p2  /               ext4    defaults,noatime  0       1
    /dev/sda1  /home/addedHDD         ntfs    defaults  0       0   #add hdd


## WIFI 설정

유선의 경우 선을 꼽고 ifconfig로 나오는 IP로 바로 접근 가능
무선랜의 경우 무선랜카드를 꼽고, 다음과 같이 설정한다.

    #달려있는 USB장비를 확인
    raspberrypi:~$ lsusb
    Protocol spec without prior Class and Subclass spec at line 17454
    Bus 001 Device 002: ID 0424:9514 Standard Microsystems Corp. SMC9514 Hub
    Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
    Bus 001 Device 003: ID 0424:ec00 Standard Microsystems Corp. SMSC9512/9514 Fast Ethernet Adapter
    Bus 001 Device 004: ID 0411:0157 BUFFALO INC. (formerly MelCo., Inc.) External Hard Drive HD-PEU2

    #IP는 다음과 같이 설정
    raspberrypi:~$ cat /etc/network/interfaces
    auto lo
    iface lo inet loopback

    # wlan setting
    allow-hotplug wlan0
    auto wlan0
    iface wlan0 inet static
    address 192.168.0.XX
    gateway 192.168.0.1
    netmask 255.255.255.0
    wpa-ssid "YOURSSID"
    wpa-psk "YOURPSK"

    # eth0 setting
    auto eth0
    iface eth0 inet static
    address 192.168.0.XX
    gateway 192.168.0.1
    netmask 255.255.255.0

IP등 변경시 정상적으로 적용하기 위해서는 다음과 같이 network서비스를 재시작한다.

    $ sudo /etc/init.d/networking restart

그밖의 명령

    #무선랜down
    $ sudo ifdown wlan0
    #무선랜up
    $ sudo ifup wlan0

# 기타 활용

## samba 설치

참고 - [라즈베리파이 활용기-NAS로 활용하기(1)](http://ghost9087.tistory.com/2)

    # samba 설치
    $ sudo apt-get install samba samba-common-bin
    # samba.conf 파일 수정
    $ sudo vi /etc/samba/smb.conf

    #Share Definitions 하위의 내용을 다음과 같이 수정
    [profiles]
       comment = Users profiles
       path = /home/addedHDD
       guest ok = no
       browseable = yes
       writable = yes
       create mask = 0777
       directory mask = 0777

     #samba 계정 추가
     sudo smbpasswd -a [username]

     #서비스 재시작
     sudo /etc/init.d/samba restart

## transmission설치

참고 - [라즈베리파이 활용기-NAS로 활용하기(2)](http://ghost9087.tistory.com/3)

    $ sudo apt-get install transmission-daemon
    #설치 후 바로 실행되는데 멈춘 후
    $ /etc/init.d/transmission-daemon stop
    #설정파일 수정
    $ /etc/transmission-daemon/settings.json
    #수정한 항목
    #download-dir
    #incomplete-dir
    #rpc-password
    #다시 시작
    $ /etc/init.d/transmission-daemon start

이후 http://localhost:9091 로 접속(transmission/YOURPASSWORD)



# 이슈

- 전원 - 외장 2.5 인치 하드 등 전원을 많이 필요로 하는 USB 연결시 5V,2A 이상의 전원을 사용해야 함, 그렇지 않으면 기동시 문제가 생긴다. 아무 핸드폰 충전기로 다 되는 건 아님

- 무선 랜 카드의 경우 IPtime에서 나오는 걸로 2개(iptime n105ua, n100mini) 붙여봤는데, n105ua는 랜카드 인식이 안되고(커널 단 문제라고...), n100mini는 정상 인식하나, 수신률이나 속도는 좋지 않은 편이다. 유선이 가능하다면 세팅 후 유선으로 사용하는 게 좋을 것 같음.

- 케이스가 아크릴로 별도로 만들어진 제품을 사용중인데, 그다지 퀄리티가 좋지 않다. 사용하다 보면, USB선이 계속 빠져서 전원이 나가는 경우가 계속 생김. 확인하고 구입해야 할 것 같다.
