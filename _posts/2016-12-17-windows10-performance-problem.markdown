---
layout: post
title: Windows10 속도가 느려질 때
date: 2016-12-17 22:27:00
tags:
- Life
- Tech
---

Windows 10 사용시 속도가 느려질 때 해결방안

집에 쓸만한 윈도우 환경이 없어서, 기존 ubuntu로 사용중이던 x201 노트북에 Windows10을 다시 설치하였으나, 사용중인 환경 구성을 위해 하나 둘 설치하다 보니, 어느새 못쓸 정도로 느려져서, 해결을 위해 다음과 같은 조치들을 실행


### Windows Defender 종료

웬지 모르겠지만, 가장 큰 성능체감의 주범인 듯, 끄니 쾌적하다.

다음과 같이 예외를 등록하면 CPU를 지속적으로 먹는 문제는 해결된다고 해서 적용하니 잘 되지 않음

    설정 > Windows Defender > 제외사항 추가
    --> 다음과 같은 파일과 폴더들을 제외처리
    C:\Program Files\Windows Defender
    C:\Program Files\Windows Defender\MsMpEng.exe


아예 설정에서 끄거나(다시 켜진다고 되어 있음.), 아래와 같이 정책 편집기에서 종료

[Windwos Defender(윈도우 디펜더) 끄기 또는 실행](http://slic.tistory.com/538)

    실행 창 띄운 뒤 gpedit.msc 실행
    컴퓨터 구성 → 관리 템플릿 → Windows 구성요소 → Windows Defender 하위의 Windows Defender 끄기를 더블클릭 하여 사용으로 바꾸어준다.


### 기타

다음 서비스 종료 - 사용과 관련이 없을 경우(서비스 관리에서)

- superfetch, Windows Search
- 홈 그룹 비활성화
- p2p Networking관련 서비스 비활성화

작업관리자에서 비활성화 하고 싶은 시작프로그램 사용 안함으로 변경
