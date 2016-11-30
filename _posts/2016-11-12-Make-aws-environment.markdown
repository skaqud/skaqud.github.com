---
layout: post
title: AWS에서 개발환경 구성하기
date: 2016-11-12 21:49:00
tags:
- Tech
- AWS
---

AWS에서 개발,검증,운영 등 새로 환경을 구성할 때 기억해야 할 사항들

# AWS구성요소

## 순서

1. 기본 OS이미지 생성 : 외부에서 접속한 뒤 ssh port변경, 보안설정 적용, 계정 생성, 이미지에 일괄 적용되어야 하는 등의 작업을 수행

2. 서버 레이어 별로 subnet을 분리(용도에 따라), 외부로 접근할 필요성에 따라 Public subnet과 Private subnet으로 구분한다.

3. vpc 생성 및 subnet(public,private subnet), router table, internet gateway 생성

4. 기본 이미지를 기반으로 jumphost 및 각각의 인스턴스 생성, Security Group과 Network ACL을 설정하며 테스트한다.

5. 준비된 인스턴스에 어플리케이션 설치 및 테스트


## EC2

- 기본적으로 AWS에서 사용하는 인스턴스, 특정 사이즈 이상으로는 금액이 기하급수적으로 커진다.

- private ip의 경우 인스턴스가 다시 시작되면(shutdown-reboot) 변경될수도 있다. 다만  reboot시에는 안바뀔수도 있음. vpc안에서는 고정됨

- 종료시 instance type을 변경 가능함. 처음에는 작은 사이즈로 올려놓고 나중에 사이즈를 변경하는 전략이 가능하다. 얼마나 아낄 수 있을지는 모르겠지만.

- 리전마다 지원 가능한 인스턴스의 Type이 약간씩 다르다. 예로, 한국 리전에는 현재 아직 고성능 컴퓨팅을 위한 P2인스턴스는 아직 없음. 타입은 계속 변경되는 것 같다. 예전에는 m1, m2 관련타입들이 많았는데 이젠 m3,m4밖에 안보인다.(네이밍으로 보면 버전이 올라간 것 같긴 하다.)

## VPC(Virtual Private Cloud)

- VPC는 처음 접할 경우 다소 설정이 복잡하나 매뉴얼이 잘 되어 있는 편임, [AWS VPC 매뉴얼-기본 VPC 및 서브넷](http://docs.aws.amazon.com/ko_kr/AmazonVPC/latest/UserGuide/default-vpc.html), 매뉴얼은 한글보다 영문이 더 잘 되어 있음.

- VPC안에서의 IP는 변경되지 않음, subnet이 나뉘어도 SG,NetACL 등의 설정에 따라 PrivateIP로 통신은 가능

- ELB는 Internal ELB와 External ELB로 구분, 내부에서 사용시 Internal을 사용

- Security Group은 80,443 이외에는 Any Open은 허용하지 않음
Network ACL은 Black List(막아야 할 곳만 막는 용도), SG는 White List(허용하는 용도)형태로 사용. SG는 stateful(요청의 in/out을 하나의 상태로 보아 in이 허용되면 out도 가능), network acl은 stateless함.(in/out을 별도로 각각 처리해야 함.)

- Private subnet에서는 외부와 통신이 필요할 경우 NAT 인스턴스나, Proxy를 이용,
(NAT를 사용할 경우 HA필요(이중화))

- DB의 경우 RDS를 사용하면 같은 vpc안에 구성할 수도, 다른 vpc에서 구성할 수도 있음, vpc안에 구성할 경우 DB 레이어만 별도의 서브넷으로 분리하고 was등에서만 접근할 수 있도록 SG를 구성해야 한다.

- 보통은 Jumphost, Bastion host등의 인스턴스를 별도로 두어 외부에서 접속할 수 있도록 하고 그 인스턴스를 통해 VPC안의 인스턴스에 접근하도록 구성


## ELB

- 생성 전에 미리 security group이나 네트워크 설정은 되어 있어야 함(물리적으로 연결되는 데 이상이 없어야 함)

- ELB는 v2가 새로 생겼으며, v2는 일반적으로 L7 스위치와 동일하게 동작하는 것 같다. 기존 classic lb의 경우 L4와 포지션이 비슷하다. v2의 경우 WEB앞에 두려고 할 때 2개 이상의 서브넷에서만 동작 가능, 기존 elb는 HA를 위해 권장하기는 하나 2개이상 서브넷의 사용을 강제하지는 않음.

- 생성 후 Health Check의 경우

    : Unhealty threshold - 해당 수만큼 응답이 없을 경우 instace를 OutOfService 처리, 2->5로 수정
    : Healthy threshold - 해당 수만큼 응답이 있으면 instance를 InService 처리, 10->5로 수정

- 인스턴스를 붙였을 경우 OutOfService가 뜨면 설정시 떼었다가 다시 붙이면 서비스 되는 경우가 있음(간혹 AWS상에서 다른 설정이 잘 맞아도 잘 안올라오는 경우가 있는 것 같음)

## RDS

- OS 없이 띄워진 DB인스턴스라고 보면 됨. OS가 없어서 간단해진 점도 있으나(설치 필요X), 기존 환경에 익숙한 사람들에게 좀 생소할 수 있음, 예 oracle 의 DB parameter 설정 등도 sqlplus상에서 불가능하며 RDS 웹 콘솔에서 해야 함.

- subnet group이라고 DB에 대한 접근제어를 위한 VPC 요소같은 게 따로 있다. VPC안에서는 subnet과는 별개이며, 그냥 설정만 해 주면 됨.

- Aurora DB는 AWS에서 만든 Mysql 호환되는 클라우드 DB인듯. 운영중인 MySQL, MariaDB 기반 대규모 서비스에서 Scaling이나 HA를고민할 필요가 없을 것 같다.

- RDS에서 생성하는 Oracle의 경우 BYOL은 라이센스를 별도 입력하겠다는 의미, license가 include된 버전의 경우 시간당 0.4달러로 2배 가까이 비쌈. -> Oracle SE One, Two 가 해당 서버들임.


## 기타

예전에는 AWS상에서 인스턴스 생성시 EC2 인스턴스가 VPC밖에 생성 가능했던 것 같은데(정확하지 않음), 현재 생성해보니 EC2생성시 자동으로 디폴트 VPC에 생성됨.
--> "2013년 12월 4일 이후에 AWS 계정을 만든 경우에는 EC2-VPC만 지원됩니다. 이 경우 각 AWS 리전에 기본 VPC를 갖게 됩니다."라고 함.


# Example

다음과 같은 간단한 1 public+1 private subnet을 가진 VPC환경에 대한 구성 방법, 매뉴얼은 다음이 가장 가까울 것 같다.

[시나리오 2: 퍼블릭 서브넷과 프라이빗 서브넷이 있는 VPC(NAT)](http://docs.aws.amazon.com/ko_kr/AmazonVPC/latest/UserGuide/VPC_Scenario2.html)

다음과 같은 순서로 작업해준다.

- AWS 콘솔에서 VPC 대쉬보드를 띄워 Your VPCs 메뉴에서 VPC를 하나 생성한다. 생성시 CIDR block에 따라 안의 서브넷에서 사용 가능한 ip갯수가 결정된다.

- 해당 VPC안의 서브넷을 두 개 생성한다. 편의상 서브넷은 CIDR을 10.0.0.0/16, public subnet은 10.0.10.0/24 private subnet은 10.0.20.0/24 로 만든다.

- public subnet에서의 외부 통신을 위한 Internet Gateway를 생성한다. vpc안에서 외부로 통신하기 위해 생성되는 Gateway라고 생각하면 되며, 단순히 태그만 입력하면 된다.

- 서브넷끼리의 통신을 위한 Route Tables 을 설정한다. public subnet의 경우 미리 생성해 준 igw(Internet Gateway)를 보도록 하고, Private subnet의 경우 외부와 직접 연결되지 않도록 public subnet과만 연결되도록 설정한다.

- 보안을 위해 Network ACL 혹은 SG(Security Group)을 설정한다. 섞어서 설정할 경우 나중에 관리가 힘드니, 경우나 상황에 따라 설정해 사용한다. ACL의 경우 H/W 방화벽에 가깝고, in/out bound를 별도로 설정해줘야 하며 stateless라 들어온 요청이 나가도록 설정되어 있지 않으면 연결이 성립하지 않는다. 서비스포트 이외의 임시포트(Ephermeral port)를 사용할 경우 일일히 설정해 줘야 한다. SG의 경우 S/W 방화벽에 가까우며, statuful 하며 들어온 요청이 outbound에 없어도 연결이 성립한다. 꼭 동시에 설정해야 한다면, ACL의 경우 black list, SG의 경우 white list로 역할을 나누어 사용하는 게 바람직하다.

- Public subnet에 jumphost를 하나 생성하고, EIP를 부여하여 해당 jumphost를 통해서만 다른 인스턴스에 접근할 수 있도록 한다.
