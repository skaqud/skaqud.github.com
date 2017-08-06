---
layout: post
title: 재야의 고수로부터 듣는 모던 아키텍처 이야기
date: 2017-07-10 01:09:00
tags:
- Tech
- GCP
- Review
---

Google Cloud Day in Google Campus(재야의 고수로부터 듣는 모던 아키텍처 구축 이야기) 내용 및 참석후기

나중에 시간이 나게 되면, 정리가 필요할 것 같음, 2,3번째 session이 인상깊었음

### 머신러닝 시작하기 - 조대협

머신러닝이란 - 학습, 평가를 반복 수행 -> 원하는 결과가 나올 때 까지
배울 수 있는 과정 - Cousera, 홍콩과기대 김성훈 교수님?
Azar앱 - hyper connect -> speech api + translate api?
tensorflow - tf.contrib -> framework을 이용

머신러닝 시스템 개발 프로세스
데이터 수집/저장 - 데이터 분석 - 데이터 전처리 - 모델 및 테스팅 - 학습 - 예측
리밸런싱 - 원본 데이터의 사이즈를 조절해 줌(머신 러닝을 위해서는 학습 데이터가 균형이 맞아야 함.)

결론
- 모델을 만드는 것 보다 활용하는 아이디어가 중요. 사용을 잘 해야 함.
- 모델은 중요하지 않다(일부일 뿐), 전체 파이프라인을 구축하고, 전체 컴퓨팅 환경도 알아야 함.
- 많은 양의 데이터를 확보, 라벨링

### couchgram - 구글 앱엔진을 통한 No-Ops

서비스에 대한 설명 - 전화(Call)기능을 기반으로 한 부가서비스

AWS에서 기존 서비스를 사용중이었음, 글로벌 서비스 - AWS 매니지드 서비스에서의 장애, 비용 이슈로 CGP로 옮김

AWS -> 클라우드를 쓰더라도 운영 비용이 있어서 전환
  - Redis Labs - Elastic Cache 대체, GCP에서 지원을 안해서 변경함
  - 로그를 모으는 역할 - firebase로 변경

사용중인 관련 도구들(3rd-party)
- asana?
- backhub - github의 백업
- google drive 의 백업 - dropbox
- dockon-monitor - 서버를 모니터링 해서, 문제가 있을 경우 전화,SMS로 알려줌, 500ms 이상이면 전화가 오도록 설정

orient db -> rdb로 복귀, 운영상의 이슈로 인해

장단점
- 웹 콘솔로 별도의 장비 없이 바로 웹 화면에서 접근 가능 -> 운영상황이 아니라면 별로 필요가 없을 것 같다.
- AppEngine도 일종의 Serverless Architecture 인듯.
- Document가 부족한 편(AWS에 비해)
- 비용이 1/5 정도 절감?, 많이 쓰면, 오래쓰면 비용이 절감됨

이전 팁

- 직접 구축 보다는 관리형 서비스를 사용하는 게 효율적이고 생산적
- 최대한 콘솔 툴에서 처리 - 속도 차이가 많이 남(웹에 비해)
- 언어 선택 잘해야 - 레퍼런스가 많은  Java, Python등

TODO
- AppEngine에 java로 deploy 테스트

### 구글 빅쿼리로 데이터 웨어하우스 구축하기 - 요기요, 배달통 데이터 팀

데이터 수집 엔지니어 입장에서의 사례, 삽질기 공유

원본 데이터가 - 회사 사정상 Postgres, MariaDB, MySQL, Oracle 등 다양, 운영 DB의 slave의 replica임, embulk 의 ETL 엔진을 사용중, 빅쿼리를 통해 데이터를 정제하여 올려줌, 데이터 통계 결과를 taeblau에 뿌려서 보게 됨.

설계 주안점 - 1)항상 같은 결과(멱등성) 2) 있는 그대로를 이용,변경을 가하지 않음(무위) 3) 단순

기존 RDB에서 변경된 데이터만 올리는 게 아니라, 전체를 올려야 하는지?

빅쿼리 테이블 종류 - 그냥 Table, 일자별 파티션 테이블
데이터를 가급적 쪼개 놓아야 비용측면에서 좋음

결론
데이터 증분, 감분, 변화분 처리
ETL엔진 제작 시도(embulk를 엔진으로 대체)
민감정보를 위한 익명화 처리 작업

제안 - Micro Warehouse Architecture - MSA를 이용한 아키텍처 구성

Data Studio < Taeblau?


### 파이어베이스를 이용한 모바일 앱 개발 - 캐쉬슬라이드(NBT)

도입 배경 - 개발 리소스 부족으로 파이어베이스를 연동하여 일부 기능을 서버리스 아키텍처로 구현
주요 기능
  - 앱 빌드 및 테스트 - 실시간 DB, 인증, 호스팅, 클라우드 스토리지...
  - 잠재고객 증가 및 참여유도 - Google Analytics, 클라우드 메세징...
안드로이드 앱 쪽에서 요청마다 키를 남겨야 함 -> firebase에서 대쉬보드 형태로 볼 수 있음.

- 구글 Analytics와 연동하여 고객 정보를 분석하고 조회할 수 있음.
- FCM 클라우드 메세징
- 회원가입 기능 지원 : token refresh time - 1시간 -> 변경 가능

랜딩페이지 등에 대한 호스팅용으로도 사용 가능

### Google Container 엔진 기반 배포 - Shaker

서비스 소개 - 비디오 제작 웹 기반 플랫폼

Kubernetes - 현재는 linux foundation에 구글이 기부한 상태
GCE기반으로 클라우드 서버 환경 사용법에 대한 설명

Pod - 가장 작은 컴퓨팅 유닛, 한개 혹은 이상의 컨테이너, 휘발성
Deployment - Pod를 배포, 관리해 줌

Deployment spec을 작성하여 어플리케이션을 배포, 롤링 업데이트 해 줌
Node Pool

Preemptive Node Pool - 언제 삭제될 지 모르는 VM을 이용, 비용이 엄청 저렴

Datadog-agent를 이용한 모니터링

로깅도 컨테이너 생성시 켜면 바로 조회 가능함

Stackdriver를 통한 로그 관리


### 서버리스 아키텍처 - 꿈많은 청년들

Cloud Function - 낮은 성능, 이벤트 기반의 비동기 서비스 , node 만 지원하고 있음
데이터는 cloud storage에 저장
용도에 맞게 적절히 사용할 것 - 예) 실시간 이미지 생성, 바코드, QR코드 생성 등, 썸네일 생성/리사이징
파일이 업로드 되어 있을 때 백업, 보관 등, 서버 장애시 Pub/Sub을 통해 slack으로 공유되도록 처리
--> 보관하기 위한 데이터 가공
