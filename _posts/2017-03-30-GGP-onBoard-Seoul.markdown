---
layout: post
title: Google Cloud On board 후기
date: 2017-03-30 23:25:00
tags:
- Tech
- Conference
- Google
- GCP
---

Google Cloud On board 후기

[행사소개](https://cloudplatformonline.com/gcp-kr-googlecloudonboard.html?utm_medium=email&utm_source=kronboard&utm_term=outbound-team&utm_content=email1-sbc&utm_campaign=2017-gc-field-eventsgcp-partner-other-apac_onboard_korea_sbc)

개인적인 생각이나, 클라우드 서비스에서는 그동안 IaaS 기반의 AWS가 강세였고, 이를 구글도 잘 알고 있었을 거라고 봄. 해서 절치부심하여 기존 앱엔진 기반의 서비스를 보완하고, 띄우기 위해 기획된 행사였던 것 같다. AWS행사처럼 밥도 잘 주고. AWS에서만 되던 것들을 GCP에 많이 보완한 듯 하고, Bigtable, tensorflow, kubernetes등 구글의 오픈소스로 공개한 원천기술과, Vision API등 구글만이 가지고 있는 것도 많이 녹여낸 것 같다. 
사실 Compute Engine과 같은 IaaS도 GCP안에 있는지 처음 알게 된 듯(앱엔진인 PaaS만 있는지 알고 있었음.). Cloud shell등 구글의 덕력이 느껴지는 포인트가 중간중간에 있었음. 

-----


모듈 1. 트랜드 변화, 빅데이터, 머신 러닝
조대협 

세션의 핵심 - 아이디어, 전략을 가지고 적절하게 구글 클라우드 플랫폼을 활용하라
도구성 서비스들은 빅 벤더 위주로 점점 더 사용하기 쉬워지고 있음, 사용자들은 전략과 아이디어로 사용하기만 하면 됨

1) No Ops를 통한 애자일 개발
- 매니지드 서비스 - 설치와 운영은 클라우드가 대행하고, 조직은 개발에만 집중
- 약간의 샘플
  - 카날리아 테스트 - 새 버전으로 약간 트래픽을 옮김 -> 마이그레이션 트래픽(문제가 없으면 트래픽을 옮겨줌)
  - 로그 분석 시스템 - 초당 10만 TPS를 가진 로깅 서버를 짧은 시간 안에 만들 수 있음 
2) 데이터 분석 기술
- 빅데이터 기반 시스템은 복잡하고 구현하기 힘듬. 다양한 오픈소스 
  - 데이터를 분석하는 목적을 위해 데이터 분석 인프라를 설치 운영하는데 많은 리소스가 소요됨.(주객이 전도)
- 클라우드 기반의 데이터 분석 아키텍처 - 결과적으로 비싸고, 시간이 많이 소요되니, 클라우드를 써라...
  - 빅쿼리 기능 - 구글 클라우드 데이터 분석 기술의 기능
- 실시간 데이터 분석 필요,firebase analytics,게임 데이터 분석 - 지리정보와 연계
- 지표를 단순화, 지표는 항상 변경되어야 함
3) 머신러닝
- 지도학습, 비지도학습, 강화학습(알파고, 자율주행 자동차 등, 실제 플레이 해 보고 학습)
- 머신 러닝은 선과 데이터 사이의 가장 짧은 점을 찾는 문제, 최적의 값을 찾을 때까지 반복 연산
- TPU - 구글에서 머신러닝을 위해 만든 CPU(GPU보다 빠름)
- 딥러닝 - 사람의 뇌를 본딴 뉴럴 네트워크 구조를 본따 만듬.
  - 뉴럴네트워크에서 딥러닝으로 리브랜딩 - 캐나다 CIFAR연구소 Hinton교수
  - MOOC(글로벌 방통대)등의 강좌를 통해 아무나 딥 러닝 기반 시스템을 만들수 있음
  - 텐서플로우 - 머신러닝을 위해 구글이 만든 오픈소스
4) 기타
- 구글 VISION API - 영상을 넣으면 영상의 text+정보가 나옴 
- 구글 VIDEO API - 비디오 Frame 마다 정보를 인식하여 그 정보를 추출.
  - 오이 분류기 개인 개발자
  - 버즈아트 - 미술작품들을 VISION API에 넣고 인덱싱, 후 판매


모듈 2. - 구글 클라우드 플랫폼 소개
이준호 솔루션스 아키텍트, GCP 공인 트레이너

1) 소개
- 리전 - 대륙, 존 - 데이터 센터 , HA를 위해 멀티 존 구성을 추천
- 구글의 데이터 기술 혁신 - GFS, MapReduce, TensorFlow 등, 빅쿼리, 빅테이블, 클라우드 스토리지, 데이터스토어, Pubsub
- kubernetes -> on premise에서 구성할수도 있도록 오픈소스로 제공
- IaaS(Compute Engine) - Devops(할당한 만큼 지불,관리필요)
- PaaS(App Engine) - Noops(사용한 만큼 지불, 관리필요X)
- 프로젝트 - 리소스, 빌링의 단위가 됨. 사용자 계정의 관리 단위
- 조직 > 프로젝트 > 리소스, 리소스는 프로젝트 간에 공유될 수 있음.
- 구글 클라우드 플랫폼 콘솔, SDK
- cloud shell - vm임, 누르는 순간 클라우드에 vm이 생성, cloud sdk를 포함, 웹을 통해 CLI로 명령을 날릴 수 있음 
  - 키가 없음, 자동으로 관리됨. --> glcloud ssh명령을 통해 접속, 만들지 않아도 ssh키가 생성되고 관리됨.
  - 웹에서 이클립스의 온라인 버전인 Orion을 통해 소스 수정도 가능
  - Cloud Shell에 tmux가 기본적으로 enable 되어 있음.
- rest api - 대부분 api에 할당량 있음, Quota조정 가능(요청에 의해)
- API Explorer - 브라우저를 통해 API를 사용해 볼 수 있는 도구

2) 무료 정책 관련
- Free trial
300$ 2달정도 사용 가능, 이후 일단 프로젝트가 닫히고, 이후 사용하면 비용이 부과됨
(12개월로 연장되었다고 함)
- Always free 특정 region의 특정 서비스는 항상 무료

3) 기타
- 다음달(4.12)에 Hands-on 교육 예정
- 실제 VM사용량을 기준으로 사용자에게 최적으로 할인된 금액만을 빌링한다고 함.(사용자가 관리하지 않아도 discount됨.)
- Committed use discount - AWS의 예약 인스턴스와 같은 개념인 듯, core 개수를 구매

점심. 머신러닝 데모
최명근
Vision API, Video API 기반으로 빅데이터 검색을 하는 데모


모듈 3. Google app engine, Cloud datastore(NoSQL)
이준호 솔루션스 아키텍트, GCP 공인 트레이너

1) AppEngine(Standard Environment)
- Code만 있으면 Deploy해서 바로 사용 가능
- BP로 Snapchat을 들었음.
- 각종 언어에 대한 관리형 런타임, 오토스케일링. 배포하고 나서 바로 서비스 가능
- 무료 일일 할당량이 있음(그 금액 이상만 과금)
- SDK를 설치해서 로컬에서 개발, 배포 테스트까지 확인한 뒤 배포

2) App Engine Flexible Environment(베타) -> docker 기반의 앱 엔진 컨테이너
- 사용자가 직접 만드는 런타임
- 로컬 개발 환경에 Docker 활용(Docker Container 이미지가 생성됨)
- 앱 엔진에서 다른 구글 클라우드 서비스를 활용 가능 

3) Google Cloud Datastore
- 처음엔 앱엔진 안에 포함되어 있던 서비스임.
- 애플리케이션 백엔드용으로 설계된 데이터베이스, 
- 수십억개 행을 NoSQL형태로 저장함. 스키마 리스 디자인.
- kind(테이블 개념)
- 빅테이블과의 차이 - SQL과 유사한 GQL을 지원(쿼리언어 사용 가능,API등으로 사용가능)

* Python으로 만든 bookshelf데모 시연
- app.yaml - 앱 엔진의 configuration이 들어있음.
- 앱엔진은 프로젝트 당 하나만 만들 수 있음, 다수의 버전을 만들 수는 있음.
- pre-warming은 필요 없으나, quota는 사용량이 늘때 문제가 될 수 있음.


모듈 4. Google Cloud Platform Storage옵션
이준호 솔루션스 아키텍트, GCP 공인 트레이너

Cloud Storage, Cloud Bigtable, Cloud SQL

1) Cloud Storage
-Object Storage, RestAPI로도 bucket을 mount해서 사용할수도 있음(특성상 속도 이슈)
-관리 간편, 용량 관리 불필요, 상시 데이터 암호화(가동여부 상관없이-disk에 쓸때 암호화 되어 저장되어 있음 구글의 임시 키로)
- 등급 - Multi-regional > Regional > Nearline > Coldline(아카이빙), 단계를 나누어 목적에 맞게 사용
- 버저닝, 객체 수명주기 관리, ACL, 리전별 버킷, 오프라인 가져오기(구글이 원하는 자료를 수동 업로드해주는 서비스), 객체 변경 관리 알림
- 빅쿼리, Computing Engine(시작,스크립트, 이미지 보관), AppEngine(객체 저장소, 로그 백업 등, Cloud SQL 등과 연결 가능

2) Cloud Bigtable
- NoSQL 와이드 컬럼 데이터베이스 서비스
- HBase API로 엑세스, Hadoop 에코시스템과 호환
- 복제 저장소, 상시 데이터 암호화, ACL
- Gmail, Google Analytics 등에서 사용 
- 엑세스 패턴 - Application API(Hadoop), 스트리밍, 배치

3) Cloud SQL
- Google에서 관리해주는 MySQL + PGSQL(베타)
- 읽기/쓰기 수직확장, 읽기 수평 확장, App Engine/Compute Engine과 원활히 통합
- DNS가 아니라, IP로 제공됨.
- 현재 2세대, 처리량, 용량 증가, 저렴한 가격


모듈 5. Google Container Engine
이준호 솔루션스 아키텍트, GCP 공인 트레이너

1) 컨테이너
  - 운영체제 레벨의 가상화, 어플리케이션 코드와 운영체제의 의존성 분리
  - 프로세서 분리, 일반적으로 Docker, rtk등 구현 방법이 있음

2) kubernetes(k8s)
  - 조향타, 컨테이너 클러스터를 오케스트레이션 할 목적
  - 10년 이상 된 google 의 경험을 바탕, 다양한 클라우드 환경 지원
  - 이동 가능한 워크로드(다양한 환경에서 구현, 개방식, 모듈식 구현)
  - 롤링 업데이트-어플리케이션 업데이트 시 중단시간 없음, 오토스케일링
  - 퍼시스턴스(영구) 디스크(DB를 위한), 멀티 존 클러스터, 부하 분산
  - Container Engine은 Compute Engine과 App Engine 사이에 있음

3) 기타 
- Container Engine은 GKE라고 발음함 일반적으로 Google Compute Engine과 약어가 같아서
- 관련 서비스로 google Cloud Container Build, google Cloud Container Registry 가 있음
- 서비스 모델을 PaaS, IaaS중간 형태라, 하이브리드 로 표현
- GKE의 kubernetes 대쉬보드는 별도로 접근할 수 없으나(구글 내부에서 관리되므로), proxy를 띄워 접근하도록 할 수 있음
- kubernetes GKE labs - 쿠버네츠 안에 GKE를 활용한 랩들이 꽤 있음, 활용하면 좋음.
- Container Registry에 빌드 트리거 기능 - bitbucket, github등에 커밋하면 컨테이너 이미지를 빌드 하여 업로드
- Container Builder - 설정 파일을 읽어서 컨테이너를 빌드하는 도구


모듈 6. Google Compute Engine, Networking
이준호 솔루션스 아키텍트, GCP 공인 트레이너

1) Google Compute Engine(GCE Instance)
- 견고한 네트워크 기능
  - 기본, 맞춤 네트워크
  - 방화벽 규칙(태그 기반 방화벽 룰 설정)
  - 리전별 부하 분산(로드 밸런서, LB, GLB(L7)),하위 네트워크(서브넷)
- 고성능 CPU, 대용량 메모리, 표준 및 공유 코어 머신 
- 표준 disk, ssd, 로컬SSD(Temporary함-reboot일때는 유지됨)
  - disk size변경은 온라인 상에서 동적으로 가능
- 인스턴스 시작 script, 메타데이터
- 오토 스케일링(매니지드 인스턴스 그룹 사용시)

2) Google Cloud Networking
- 로드 밸런서 
  - L4 LB, L7 GLB, pre-warming이 없음.

3) Google Cloud CDN
- 반드시 L7 로드밸런서가 필요, enable/disable만 가능 
- 기능이 열악,
- CDN Interconnect를 이용하여 앱엔진을 통해 프로젝트를 직접 연결하도록 구성하는 게 나음

4) Google StackDriver
- 모니터링, 로깅, 진단(디버깅)을 통합
  - 온라인상에서 서비스를 직접 디버깅할 수 있음
- collectd를 사용하게 되어 있음.

Google Cloud 배포 관리자
- 인프라 관리 서비스, 하나의 인프라 셋을 yaml로 만들어 배포할 수 있도록 


모듈 7. 빅데이터
이준호 솔루션스 아키텍트, GCP 공인 트레이너

- Google에서 생각하는 빅데이터 플랫폼
  - NoOps형태로 빅데이터 플랫폼이 사용자에게 제공되는 것
- 서비스의 종류
  - 빅쿼리 
    - 매니지드 분석 데이터웨어하우스(데이터도 저장해야 함)
    - 대규모 데이터셋에 대한 실시간에 가까운 인터랙티브 제공
    - SQL과 유사한 구문을 사용한 쿼리
  - Pub/Sub : 메세징 서비스
    - Topic/subscription 모델
  - Dataflow : 일종의 ETL서비스, Apache 재단의 Bean을 실행하는 러너
    - 확정성/안정성을 가진 데이터 파이프라인
    - 사용시 임시로 GCE 인스턴스를 사용 
  - Dataproc
    - 구글에서 관리해주는 Hadoop cluster, job이 실행중일 때 cluster를 늘릴 수 있음
    - Hadoop/Spark/Hive/Pig
    - Cloud storage, Stackdriver등과 통합/연계 가능
    
cloud datalab - jupiter를 서비스로 제공


---------------------------------------------------------

관련 자료 
책 - 한빛출판사 - 빠르게 훓어보는 구글 클라우드 플랫폼 - pdf를 e-book은 무료 다운로드 가능

도입 문의 등
gcp@rockplace.co.kr

실습해 볼 수 있는 lab들
- google.qwiklabs.com
- coursera.org/googlecloud

