---
layout: post
title:  "MacOS SD카드 용량 확장"
date:   2016-04-04 21:10:49 +0700
tags:
- hobby
- life
---

mac에서 SD카드를 통해 용량 확장시, 인식을 못하거나 에러를 뱉는 경우를 방지하기 위한 방법입니다.(애초에 용량 큰 걸 샀으면...)

# sleepwatcher 설치

mac의 장점인 전원을 끄지 않고 sleep 모드로 두었을 때, 깨어날 경우 간혹 SD카드를 인식하지 못하는 경우가 있음.
jettison(http://macnews.tistory.com/2308) 이라는 대안도 있으나 소개글의 댓글을 보면 이게 되는건지 아닌지 확신할 수가 없어, 검색을 통해 sleepwatcher 라는 도구를 설치. 약간의 shell script에 대한 지식이 필요함.

[JetDrive Lite, Dropbox, 그리고 sleepwatcher](https://joshua.huh.today/2016/03/06/jetdrive-lite-dropbox-and-sleep-watcher/)

설치

    brew update && brew install sleepwatcher
    ln -sfv /usr/local/Cellar/sleepwatcher/2.2/*.plist ~/Library/LaunchAgents
    launchctl load ~/Library/LaunchAgents/de.bernhard-baehr.sleepwatcher-20compatibility-localuser.plist

script 작성

    #sleep 내용
  	#!/bin/sh
  	diskutil unmount /Volumes/SkaMacSD
  	sleep 1
  	kextunload -b com.apple.driver.AppleSDXC

    #wake 내용
  	!/bin/sh
  	kextload -b com.apple.driver.AppleSDXC
  	sleep 1
  	diskutil mount /Volumes/SkaMacSD

다만, 위와 같이 적용한다고 해도, 10중 1,2는 제대로 동작하지 않음....

# ITunes 라이브러리 옮기기

[Clien-Itunes보관함 변경](http://www.clien.net/cs2/bbs/board.php?bo_table=cm_mac&wr_id=849473)

1. 아이튠즈 설정 - 고급 - iTunes Media 폴더 위치
라는 메뉴에서 원하는 위치로 변경

2. 파인더에서 기본적인 보관함 위치인 '홈폴더 - 음악 - iTunes' 폴더를 SD카드로 이동한다음
아이튠즈를 실행할때 Option키를 누르면서 실행하면 보관함 선택할 수 있는 메뉴가 뜹니다.
여기서 옮긴 보관함을 선택
