---
layout: post
title:  "Jekyll로 Github Page에 블로그만들기"
date:   2016-07-31 22:10:00 +0700
tags:
- life
- Tech
---
개인적인 자료 정리의 목적으로, Github에서 만든 서비스 중 page에 jekyll 이라는 설치형 블로그를 설치하고 구성해 보았다.

\_로 시작하는 파일들, 디렉토리들이 많은데 편집 편의상 앞에 \를 달아놓았음.(실제는 없음)

# Jekyll 설치 및 설정

## Jekyll 설치

이미 잘 정리된 포스트를 참고하여 설치

[지킬로 깃허브에 무료 블로그 만들기](http://nolboo.kim/blog/2013/10/15/free-blog-with-github-jekyll/)

[초보자를 위한 Jekyll Blog 시작하기](http://halryang.net/Jekyll-Blogging-For-Beginners/)

[지킬 한글 번역 사이트-설치](http://jekyllrb-ko.github.io/docs/installation/)

별다르게 어려운 점은 없었음.

    #gem을 통해 installation(mac os에서-xcode Command Line Tool을 미리 설치해야 함)
    gem install jekyll

    # 블로그 생성
    jekyll new skaqud.github.com

    #시작하기
    jekyll serve --watch
    # 이후 로컬에서 http://localhost:4000 에서 확인, 변경이 바로 반영됨


추가로 사용중인 ubuntu 노트북에서도 다음과 같이 jekyll 환경을 설치

[운영체제별 지킬 설치와 사용](http://vjinn.github.io/install-jekyll/)

## Jekyll 설정

기본적인 설정은 \_config.yml 페이지 안에 있음, 블로그 명, 사용하는 플러그인... 등 관련 정보를 적절히 수정

테마에 따라 다르지만, 기본적으로 블로그 맨 밖에 있는 index.html, about.md, archive.md 등의 파일들이 자동적으로 메뉴로 생성되도록 되어 있음. 다른 github page의 페이지들을 참고하여 archive, tags 페이지를 생성.

차후 시간이 되는대로 검색 기능을 구현할 예정.

# 테마 설치 및 적용

[지킬 테마 모음](http://jekyllthemes.org)

위의 사이트에서 취향에 맞는 테마를 고른 뒤, download 하여 블로그를 시작하거나, Homepage로 가서 개발자가 github에 올려놓은 실제 구동중인 사이트 소스를 참고하여 받을 수 있음, 간혹 jekyll이나 플러그인들의 버전 문제로 정상 구동이 안될 경우도 많으므로, 사이트나, github상의 README를 잘 읽어볼 것

테마별로 약간씩, page나 메뉴를 추가하는 방식들이 다름, 사용자의 소스를 참고하여 추가하면 html에 약간 익숙한 정도라도 추가 가능, ruby나 rails에 대한 지식이 있으면 더 좋을 것 같다.

## TOC(Table of Contents) 표시하기

문서에서 상단에 보여지는 TOC를 설치하기 위해 다음과 같이 설정하였으나, 로컬에서는 동작하나, github에서는 보이지 않음(아직 실패중)

[Jekyll TOC Genetator](https://github.com/dafi/jekyll-toc-generator)

적용방법

- tocGenerator.rb 파일을 \_plugin 디렉토리에 복사(없으면 생성)
- css/toc.css 파일을 css디렉토리에 복사
- 이후 content가 들어가는 곳에 "content" 부분을 "content 파이프 toc_generator" 로 수정해주면 됨

다만, markdown문서가 #, ## 등 제목 레벨의 내용이 없다면 아무것도 보이지 않을 수 있음.

적용은 되나, 로컬에서만 보이고, github에 push 하고 난 뒤는 보이지 않음....ㅜ.ㅜ;

## 추가로 적용할 부분들

- tag page 확장
- web font 변경
- 검색 기능 추가
- 오류 수정 - TOC 기능


## Google Analytics 설정

웹 로그 분석 서비스임. 자세한 내용은 구글링을 참고.

Google Analytics 가입 후 다음과 같은 Script들을 모든 페이지 상단에 넣어주면 됨, 보통은 include되는 페이지 한 곳에 넣어주면 적용됨.

    <!-- Google Analytics -->
    <script>
      (function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
      (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
      m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
      })(window,document,'script','https://www.google-analytics.com/analytics.js','ga');

      ga('create', '발급받은TrackingCode', 'auto');
      ga('send', 'pageview');

## DISQUS 설정

Communication을 위한 댓글다는 기능, 필요성을 못 느껴 설치하지 않음.

# 트러블슈팅

- "\_site" 가 간혹 꼬이는 경우가 있음. category, tag 설정이나, 날짜별로 markdown을 jekyll 이 컴파일 해 놓는데, 이 경우 post에 대해 해당 정보를 바꿔주거나 하면 \_site 내의 정보들이 꼬이는 경우가 있음, 다음 명령어로 해당 디렉토리를 정리해 줌.

    jekyll build

- 테마 적용시, Jekyll 등의 버전 문제로 \_congif.yml의 바르지 않은 설정이 들어가 있거나, 필요한 플러그인(jekyll플러그인)이 없어서 정상적으로 시작되지 않을 수 있음. 로컬에서 구동해보고 나오는 에러메세지를 확인하고 처리.
