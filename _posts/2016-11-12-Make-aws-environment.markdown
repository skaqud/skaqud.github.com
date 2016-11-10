---
layout: post
title: AWS에서 개발환경 구성하기
date: 2016-11-12 21:49:00
tags:
- Tech
- AWS
---

(작성중)

# AWS VPC(Virtual Private Cloud) 기반의 인프라 환경 구성하기

## 순서

1. 기본 OS이미지 생성 : 외부에서 접속한 뒤 ssh port변경, 보안설정 적용, 계정 생성 등의 작업을 수행

2. 서버 레이어 별로 subnet을 분리(용도에 따라), 외부로 접근할 필요성에 따라 Public subnet과 Private subnet으로 구분한다.

3. vpc 생성 및 subnet(public,private subnet), router table, internet gateway 생성

4. 기본 이미지를 기반으로 jumphost 및 각각의 인스턴스 생성, Security Group과 Network ACL을 설정하며 테스트한다.

5. 준비된 인스턴스에 어플리케이션 설치 및 테스트

## VPC(Virtual Private Cloud)

- VPC안에서의 IP는 변경되지 않음, subnet이 나뉘어도 SG,NetACL 등의 설정에 따라 PrivateIP로 통신은 가능

- ELB는 Internal ELB와 External ELB로 구분, 내부에서 사용시 Internal을 사용

- 또한 ELB는 v2가 새로 생겼으며, v2는 일반적으로 L7 스위치와 동일하게 동작하는 것 같다. 기존 classic lb의 경우 L4와 포지션이 비슷하다. v2의 경우 WEB앞에 두려고 할 때 2개 이상의 서브넷에서만 동작 가능, 기존 elb는 HA를 위해 권장하기는 하나 2개이상 서브넷의 사용을 강제하지는 않음.

- Security Group은 80,443 이외에는 Any Open은 허용하지 않음
Network ACL은 Black List(막아야 할 곳만 막는 용도), SG는 White List(허용하는 용도)형태로 사용. SG는 stateful(요청의 in/out을 하나의 상태로 보아 in이 허용되면 out도 가능), network acl은 stateless함.(in/out을 별도로 각각 처리해야 함.)

- Private subnet에서는 외부와 통신이 필요할 경우 NAT 인스턴스나, Proxy를 이용,
(NAT를 사용할 경우 HA필요(이중화))

- DB의 경우 RDS를 사용하면 같은 vpc안에 구성할 수도, 다른 vpc에서 구성할 수도 있음, vpc안에 구성할 경우 DB 레이어만 별도의 서브넷으로 분리하고 was등에서만 접근할 수 있도록 SG를 구성해야 한다.

- RDS에서 생성하는 Oracle의 경우 BYOL은 라이센스를 별도 입력하겠다는 의미, license가 include된 버전의 경우 시간당 0.4달러로 2배 가까이 비쌈. -> Oracle SE One, Two 가 해당 서버들임.


# ELB

- 생성 전에 미리 security group이나 네트워크 설정은 되어 있어야 함(물리적으로 연결되는 데 이상이 없어야 함)
- 생성 후 Health Check의 경우
: Unhealty threshold - 해당 수만큼 응답이 없을 경우 instace를 OutOfService 처리, 2->5로 수정
: Healthy threshold - 해당 수만큼 응답이 있으면 instance를 InService 처리, 10->5로 수정

인스턴스를 붙였을 경우 OutOfService가 뜨면 설정시 떼었다가 다시 붙이면 서비스 되는 경우가 있음(간혹 AWS상에서 다른 설정이 잘 맞아도 잘 안올라오는 경우가 있는 것 같음)

# 기타

예전에는 AWS상에서 인스턴스 생성시 EC2 인스턴스가 VPC밖에 생성 가능했던 것 같은데(정확하지 않음), 현재 생성해보니 EC2생성시 자동으로 디폴트 VPC에 생성됨.
