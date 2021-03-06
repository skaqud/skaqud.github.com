---
layout: post
title: 클라우드 아키텍처 설계
date: 2019-04-12 21:03:00
tags:
- Tech
- Cloud
- AWS
---

경험에 의한 클라우드 아키텍처 설계방법 정리


## 클라우드 아키텍처 설계

### 클라우드 엔지니어의 역할

설계단계에서부터 개발초,중반 단계까지 다음과 같은 항목들을 수행함

(1) 클라우드 아키텍처 설계 - 비기능 요구사항을 만족, 클라우드 아키텍처의 비기능요소(1.보안,2.안정성,3.성능효율성,4.비용최적화,5.운영우수성-From AWS)를 만족하는 클라우드 아키텍처를 설계

    - 보안 - VPC등 외부 접속에 물리적인 망 분리, Security Group`의 관리
      - 아키텍처 구성 초반, BaseImage작업시 OS보안 준수
      - Jumphost, Bastion host등 보안구성

    - 안정성 - Failover, LB등을 활용한 서비스 연속성,안정성 유지방법
        - ELB, DNS, GLB등 적용한 리버스 프록스 패턴 활용
        - Failover, AutoRecovery
    
    - 성능효율성 - 적합한 리소스 유형, 사이즈 선택, 성능 모니터링과 적절한 정보 제공
    - 비용최적화 - 줄일 수 있는 비용을 줄이는 구성, AutoScaling, 예약 인스턴스 사용 등
    - 운영우수성 - 코드기반의 인프라(변경관리), 자동화

(2) 개발환경 구성 - 개발-검증-운영계(dev-stg-stg2-prd)

    - 개발환경, 검증환경, 운영환경별 구성   
    - 운영환경의 경우 설계원칙을 만족하는 클라우드 구성을 적용해야 함.

(3) 세부 환경구성 절차

    - 기본 OS이미지 생성 : 외부에서 접속한 뒤 ssh port변경, 보안설정 적용, 계정 생성, 이미지에 일괄 적용되어야 하는 등의 작업을 수행

    - 서버 레이어 별로 subnet을 분리(용도에 따라), 외부로 접근할 필요성에 따라 Public subnet과 Private subnet으로 구분한다.

    - vpc 생성 및 subnet(public,private subnet), router table, internet gateway 생성

    - 기본 이미지를 기반으로 jumphost 및 각각의 인스턴스 생성, Security Group과 Network ACL을 설정하며 테스트한다.

    - 준비된 인스턴스에 어플리케이션 설치 및 테스트


### 클라우드와 레거시 아키텍처 구성의 차이점

개인적으로 느꼈던 클라우드 구성시 기존과의 차이점.(사례위주, 주관적, 두서없음)

    - 자유롭게 생성/삭제 가능하므로, 빈번한 아키텍처 재구성 작업을 자유롭게 할 수 있음
        - 하드웨어 디펜던시 등 레가시와 비교하여 되돌릴 수 없는 결정이 없음, 잘못되면 다시 생성하기만 하면 됨, 매우 빠르고 신속하게 여러 환경을 생성하고 테스트할 수 있다.

    - 테스트 환경 생성(성능테스트와 같은 일회성)시 장점
        - 사용한 만큼만 비용을 지불하면 되므로, 기존에 비해 비용이 절감, 접근성 용이

    - 비용문제로 인한 최적화 작업 필요 : 기존 레거시에는 필요 없던 부분
        - 미사용기간동안 VM을 내려놓거나, 예약 인스턴스를 사용
        - AutoScaling 이용하여, 사용자 수에 맞게 인스턴스 사이즈를 조정함.

    - 클라우드 기술(요소)들은 주로 사용하는 것들만 씀(VM,ELB,RDS등등)
        - 구성요소는 매우 많고 다 알고 적용하기는 힘들다. 레거시에 맞는 기술 스택을 찾아 적용만 하기도 힘들것 같음. 다만, Severless 아키텍처 등 새로운 개념은 기술스택을 기반으로 서비스를 거꾸로 만드는 방법도 가능할 듯 하다. 

    - 기타 
        - VPC안에서 IGW,Subnet 망분리 설정은 어렵고 골치아프다. 레거시와는 또다른 클라우드 기반 지식을 요구함.
        - 클라우드 벤더만 알 수 있는 블랙박스 부분이 있다. 오류의 원인을 알 수 없는 경우가 있음.
        - HA구성시 Multi AZ, Region을 기반으로 구성하여야 효율적


### 참고

- [대용량 아키텍처 설계 패턴](https://www.slideshare.net/Byungwook/4-61487454)
- [AWS Well-Architected](https://aws.amazon.com/ko/architecture/well-architected/)
  - [한국어 번역](https://futurecreator.github.io/2018/07/13/aws-well-architected-framework/) -내용이 많지 않음


