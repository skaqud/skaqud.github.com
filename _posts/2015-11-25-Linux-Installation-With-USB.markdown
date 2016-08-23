---
layout: post
title:  "부팅 가능한 USB로 Linux 설치하기"
date:   2015-11-25 11:20:15 +0700
tags:
- Tech
- life
---

사용중이던 Thinkpad X201이 Windows 10을 설치하니 도저히 사용하지 못할 정도로 느려져서, 좀 더 가벼운 OS를 설치해보기로 했다. ODD가 없어서 USB를 통해 설치하기로 함.

준비물

1. Linux OS 이미지(보통 ISO) - 개인적인 느낌은 다음과 같다.

[ubuntu](http://www.ubuntu.com/download/desktop) - ubuntu desktop main ISO, unity기반이라 다른 제일 무거운 편이나, 개인적인 느낌으로 사용자 수가 많아 레퍼런스도 많고 사용하기 편하다는 느낌을 받았다.

[xubuntu](http://xubuntu.org/getxubuntu/) - xcfe기반, 쥐가 마스코트, unity,kde보다는 가벼움

[kubuntu](http://www.kubuntu.org/getkubuntu/) - kde기반, 디자인이 미래지향적이고 사이버틱한 느낌인데, 취향에 안맞다.

[ubuntu mate](https://ubuntu-mate.org/what-is-ubuntu-mate/) - 데스크탑 환경으로 MATE 사용, GNOME2 사용, 취향에 안맞음

[linux mint](https://www.linuxmint.com/download.php) - 가장 윈도우와 유사하며 최근 리눅스 배포판 중 1위, 하지만 역시 내 취향이 아님, 느낌이 ubuntu mate와 비슷함

[ElementaryOS](https://elementary.io) - 가장 예쁜 OS, 개인적으로 버그가 좀 잦은 편이었음, 가장 체감했을 때는 멀티모니터 사용시 메뉴바를 한쪽에서만 사용하지 못한다던지, 기본적인 사운드 출력에 문제가 있다던지 등.

위에서부터 한번씩 돌았다가 다시 맨위로 갔음, 사용자 수나, 안정성 면에서 가장 현 시스템에 나은 것 같다.

2. 설치 USB 제작 프로그램 - [rufus](https://rufus.akeo.ie/?locale=ko_KR) - 설치 필요 없고, 가벼움

3. USB - 4GB짜리면 된다.


rufus 사용시 다음 주의(나머지는 어렵지 않음)

    디스크 형식과 부팅 시스템 유형 - MBR파티션 형식의 BIOS 혹은 UEFI
    파일시스템 - FAT32

설치할 ISO를 선택하여 rufus에서 부팅디스크를 만든 뒤, 해당 USB를 꼽고,BIOS에서 부팅이 되도록 하여 설치를 진행하면 된다.
