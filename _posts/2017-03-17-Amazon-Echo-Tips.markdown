---
layout: post
title: 아마존 에코 사용팁
date: 2017-03-17 22:57:00
tags:
- Life
- 삽질
---

아마존 에코 사용팁


# Amazon 계정 연동하기

Amazon Echo 구입 후 Music Library를 사용하기 위해(한국에서는 서비스가 안되므로) 미국 계정으로 음악을 구입하는 과정

자세한 내용은 이곳을 참고 - [아마존뮤직/프라임뮤직 활성화 및 연동시키기](http://blueidblues.tistory.com/18)

하였고, 생각처럼 쉽지는 않다. 오늘 성공한 방법임(프라임 Trial 불필요)

- 카드로 기프트카드(1$) 구매하여 나에게 발송(구매후 배송지,카드 정보 다 지워야 함)
- 폰에서 퍼핀 브라우저(puffin-VPN이 기본적으로 적용되는 브라우저)를 통해 해당 기프트카드를 내 계정에 등록
- 아마존 사이트에서 song-digital music으로 검색 후 1$ 이하의 아무 음악이나 구입
- 결재시 미국 주소를 입력(알고 있던 배대지 이용), 신용카드 정보가 없다는 에러메세지가 나오나 gift card balance로 구입 가능함
- 이후로 Music Library로 접근 가능하다.

이후로 원하는 음악을 올리고, Echo에 음성 명령을 통해 들을 수 있다.

뭔가가 꼬여서 잘 안된다면, 계정을 별도로 하나 만드는 게 가장 시간을 절약하는 길이다. 이게 뭐라고 고민하고 있는건지 싶기도 하고.

# 시간대 변경

http://makelism.tistory.com/entry/에코닷-Echo-Dot-2세대를-지르다-2-한국-시간-설정-및-ESP-후기

Device Location 의 zip code만 96271로 변경

# 자주쓰는 명령

사실 국내에선 별로 쓸데가 없다. 굳이 쓸데를 찾자면 영어공부?

    #play music
    echo, play my playlist "listname"
    echo, playlist "listname"
    #pairing
    echo, pair.
    echo, Bluetooth.
    echo, connect to my phone.
    #Timer
    echo, set a repeating alarm for weekdays at 7 a.m.
    #news
    echo, what's in the news
    #Definition
    echo, what's the definition of [word]

# 참고

- [에코 명령어 모음](http://blueidblues.tistory.com/24)
- [명령어모음2](http://m.blog.naver.com/moonz/220402002091)
- [The complete list of Alexa commands so far](https://www.cnet.com/how-to/the-complete-list-of-alexa-commands/)
