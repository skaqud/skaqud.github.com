---
layout: post
title: Cloud Foundry
date: 2018-03-23 11:21:00
tags:
- Tech
- CF
---

Cloud Foundry에 관한 정리

# 소개

기본적으로 오픈소스 PaaS플랫폼. 현재까지 버전은 v1,v2가 있으며 오픈소스인 프로젝트를 각각의 벤더(Pivotal, IBM, AWS, GCP..)등에서 각각의 형태로 커스터마이징(?)하여 서비스중임. 서비스 형태는 PaaS형태도 있고, IaaS에 얹어서 Private하게 설치하여 사용할 수도 있음. 버전별로 전체 아키텍처는 크게 다르지 않으나, 벤더별로 각각의 아키텍처를 가져다 사용하나 수용하고 있는 최신 버전의 차이가 있음(예를 들자면 diego에서 docker의 지원여부 등)

처음 접근시 파악하기 힘든 부분이 있는데, Pivotal Web Service에 계정을 만들고 Get Started 부터 차근차근 해 보는 게 좋을듯. 오픈소스 버전의 경우 UI가 없음.

아키텍처는 단순하게 예를 들자면 아래와 같음-[참고:cloud-foundry-v2-beginner](https://www.slideshare.net/jacopen/cloud-foundry-v2-beginner),

- Cloud Controller - API를 제공하고 각각의 구성요소를 제어
- Router - API와 Apps에 대한 접근을 전달
- DEA - 사용자 Apps를 실행
- Health Manager - 사용자 Apps의 Health를 모니터링
- NATS - 각각의 구성요소를 연결하는 Lightweight messaging system.

![cf1](https://user-images.githubusercontent.com/2344830/38226116-ec831590-3732-11e8-9519-b68f5828660e.png)


# 용어설명

구성요소들의 명칭에 대한 설명


|용어 | 설명|
|:------|:-------|
|cf-deployment(cf-release) | canonical manifest for deploying the CF Application Runtime without the use of cf-release. cf-release가 deprecated되고, 이를 대체함. cf-release에 비해 보안이 좀 더 강화되고, 이전 버전의 DEA를 지원하지 않는 등 차이가 있다.(공식github:https://github.com/cloudfoundry/cf-deployment) → deployment guide : https://github.com/cloudfoundry/cf-deployment/blob/master/deployment-guide.md|
|bosh| bosh outer shell, 대규모 분산 서비스들의 배포, 관리를 위한 툴체인 |
|bosh-lite| bosh의 lightweight버전으로 별도의 VM 없이 vagrant 환경에서 system을 deploy할 수 있음|
|micro-bosh| vm으로 구성된 bosh로, CF를 deploy하거나 IaaS상에 소프트웨어를 deploy할 수 있음|
|diego | Diego is a self-healing container management system that attempts to keep the correct number of instances running in Diego Cells to avoid network failures and crashes. 컨테이너 환경에서 컨테이너의 집합을 관리해주는 기술인듯.|
|stemcell| bosh에서 사용하는 OS이미지. 기본적으로 minimum os와 bosh agent등이 미리 설치되어 있다고 함|
|buildpack| Heroku에서 개발, Buildpack은 중재된 언어, 프레임워크 실행환경을 제공한다. detect-compile-release의 구성으로 되어 있는 어플리케이션 실행을 위한 metadata와 코드의 모임이다. docker의 Dockerfile 과 유사한 역할|
|warden| Pivotal CF의 컨테이너 기술, 컨테이너 집합을 관리하는 서비스, server/client간의 메세지를 관리하는 프로토콜을 제공.|
|garden|warden의 ruby,bash부분을 go로 재작성(더 나은 버전), diego와 함께 사용하여 docker 이미지를 올릴수 있다고(?)|
|DEA|Droplet Execution Agent, User application이 실행되는 런타임 환경, v1에서는 DEA, v2에서는 dea_next로 표현. 그 다음버전은 diego|


# Get Started

간단하게 cf cli를 설치하고 spring-petclinic을 Pivotal CF에 올리는 과정을 정리

- 미리 준비해야 할 것 : linux(Ubuntu 14 or 16), SunJDK 1.8이상

우선 [요기](https://pivotal.io/platform/pcf-tutorials/getting-started-with-pivotal-cloud-foundry/introduction)를 참고해서 cf-cli를 설치

설치하고자 하는 어플리케이션 war(jar)를 빌드 - [spring-petclinic](http://projects.spring.io/spring-petclinic/)

    $ git clone https://github.com/spring-projects/spring-petclinic.git
    $ cd spring-petclinic
    $ ./mvnw clean install

target디렉토리 하위에 생성된 jar를 cf에 push하면 됨.

PCF에 로그인한 뒤(PWS계정 필요)
cf login -a https://api.run.pivotal.io




# 참고자료

- [소개(제타위키)](https://zetawiki.com/wiki/클라우드_파운드리)

- [소개(2012년 자료)](http://www.mimul.com/pebble/default/2012/05/22/1337665188713.html)

- [cloud foundry 설치(2013)](https://www.slideshare.net/nanhapark/deview-2013-backend-paas-cloudfoundry)

- [pass-ta](https://paas-ta.kr)

- [Cloud Foundry | How it works](https://www.slideshare.net/jacopen/cloud-foundry-v2-beginner)

- [Cloud Foundry V2 | Intermediate Deep Dive](https://www.slideshare.net/jacopen/cloud-foundry-v2-intermediate)

- [Cloud Foundry Technical Overview](https://www.slideshare.net/cdavisafc/cloud-foundry-technical-overview)

- [CF Sample Application](https://github.com/cloudfoundry-samples)



