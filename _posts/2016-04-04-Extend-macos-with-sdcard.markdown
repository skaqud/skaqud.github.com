---
layout: post
title:  "MacOS SD카드 용량 확장"
date:   2016-04-04 21:10:49 +0700
categories: hobby, life
---
## mac SD 설정 관련 ##

sleepwatcher 설치

참고 : https://joshua.huh.today/2016/03/06/jetdrive-lite-dropbox-and-sleep-watcher/

	brew update && brew install sleepwatcher

	ln -sfv /usr/local/Cellar/sleepwatcher/2.2/*.plist ~/Library/LaunchAgents

	launchctl load ~/Library/LaunchAgents/de.bernhard-baehr.sleepwatcher-20compatibility-localuser.plist

	sleep 내용
	#!/bin/sh
	diskutil unmount /Volumes/SkaMacSD
	sleep 1
	kextunload -b com.apple.driver.AppleSDXC

	wake 내용
	!/bin/sh
	kextload -b com.apple.driver.AppleSDXC
	sleep 1
	diskutil mount /Volumes/SkaMacSD



## ITunes 라이브러리 옮기기 ##

참고 : http://www.clien.net/cs2/bbs/board.php?bo_table=cm_mac&wr_id=849473

1. 아이튠즈 설정 - 고급 - iTunes Media 폴더 위치
라는 메뉴에서 원하는 위치로 변경하면 됩니다.
하지만 이경우 아마도, 새로운 보관함을 만드는 것일겁니다 아마도.

2. 파인더에서 기본적인 보관함 위치인 '홈폴더 - 음악 - iTunes' 폴더를 SD카드로 이동한다음
아이튠즈를 실행할때 Option키를 누르면서 실행하면 보관함 선택할 수 있는 메뉴가 뜹니다.
여기서 옮긴 보관함을 선택해주면됩니다.
