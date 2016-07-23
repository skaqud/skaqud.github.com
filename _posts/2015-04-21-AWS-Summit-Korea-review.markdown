---
layout: post
title:  "AWS Summit korea 참관후기"
date:   2015-04-21 16:10:49 +0700
categories: Tech 후기
---

AWS Summit korea에서 들었던 내용들을 시간순서대로 적어보았습니다.

---


SM엔터테인먼트 on AWS
---

잘 기억이....ㅡ.,ㅡ;


AWS 최신 서비스 살펴보기 - Aurora, Lambda, EFS, Machine Learning, ECS
---

변화
기술 : SOA - MicroSerivces
조직 : Decentralized 아주 작은 팀들이 필요,  Agility, Ownership --> Devops
   ---> two pizza teams

APOLLO - 배포 서비스 도구
pipelines - Continuous Delivery 도구
---> 둘을 이용한 Devops pipeline

천개가 넘는 팀 + MS  ===> 50 Million Deployment

Code Commit, Code Deploy, Code Pipeline
EC2 + docker --> 수천개의 container???? ----> EC2 Container Service

AWS Lambda ??????????? -- 코드를 직접 올려놓으면 바로 실행됨, 서버 없이, S3기반으로????

Machine Learning - 똑똑한 어플리케이션? ---> 한계 - 비용이 너무 많이 듬, 전문가가 없음 --> AWS Machine Learnig을 통해 극복???
사용하는 방안? 배치를 통해 redshift,S3에 넣고 사용, 혹은 api등을 통한 실시간 사용

RDS Aurora - 기존의 RDBMS 데이터구조로는 폭발적으로 증가하는 데이터를 유지하기 힘듦. 기존에는 RDB가 Monolithic 구조 -> sharding 도 구조적으로 복사하는 것 이상은 아님 -> 결국 disk를 share하고 사용
-----> 기존 구조가 고비용이고 유연하지 않다고 봄, 가용성 문제가 있음.
Aurora - 3개의 AZ에 저장, 한 AZ의 두군데에 저장되며, SSD를 사용하여 속도가 빠름. 64TB까지 사용할 수 있음. --> Self healing, Fault tolerant 함


shared file storage 운영은 아주 힘든일, demand에 따라서 다름 --> 요청을 예상할 수 없어서....
--------> EFS(elastic File system) : simple, elastic, scalable

NORMA?


국내 사례로 본 클라우드 운영 최적화
---


모니터링, 자동화, 빌링
MEGAZONE - AWS Cloud Partner

고객사례 Use Cases
전자 - 스트리밍, 영상 분석 서비스 - 단일 Ec2에서 사용시 유휴리소스 낭비 등 문제 --> Docker Container기반 마이크로 단위 서비스화  -> ECS서비스 사용(EC2 리소스 사용 극대화)

글로벌 e러닝 서비스 - SLA 99.99%, 보안 기준 준수, 이중화, Legacy연동
-- IDC기반으로는 고려할 사항/비용이 너무 많이 듬
---- Multi AZ 기반으로 구성 NAT/WEB/WAS/DB이중화, SES사용

N게임사 Multi-region 아키텍처 ---> 미주에 특화된 아키텍터
---> 이중화 VPN 연동,

G게임사 사례

---> 한국어 기술지원 가능

Blended(혼합-시간+전체사용료), Unblended(종량-사용시간으로 산출) Cost ?????? RI를 구매했는데 사용료가 오름??? --> 총 금액은 같으나, 부서 내부적인 금액은 조금씩 다를 수 있음
(콘솔상에서 보이는 금액은 Blendid 라고 함) ---> 빌링솔루션


비용최적화
--> 인스턴스 type 최적화 - Scannig - Detecting - Patterning - Recommendation --> 비용 절감안 제공


엔터프라이즈에서의 하이브리드 환경 전략
---

기업은 더 이상 느리게 일할 수 없습니다.
빠른 의사결정에 맞는 빠른 실행이 필요
성공을 위해 더 많은 실험이 필요-기존 인프라로는 충분하지 않다.(고투자,고비용), 실패비용이 높아서 실험을 할 수 없다. ---> 이미 투자한 인프라가 있을 경우 어떻게?
------> 하이브리드 환경 전략(투자 보호 + 혁신)

하이브리드 환경이란??? - 데이터센터와 AWS 클라우간의  네트워크, 데이터 공유
---> AWS를 기존 기업 데이터센터의 확장으로 사용
AWS의 VPC와 연결 or VPN으로 연결 or Direct Connect 라는 방식이 있음.(전용선으로 Direct Connect location에 연결)
데이터 저장 S3,Storage Gateway - 데이터 백업(Glacier)

DR검토
---> DR site로서의 AWS - Store&Backup, Pilot Light(데이터만 백업하고, 인스턴스는 inactive 상태로), Warm Standby(인스턴스는 작게 구성하고 이후 늘리는..), Multi Site(즉각적인 사용 가능)

데이터 분석 서비스

핵심 업무에도 사용 - Dole:Sharepoint 사용, SAP HANA 사용(켈로그)


엔터프라이즈 클라우드 도입 및 고려사항
---

- 메가마트 사례(NDS)

클라우드 - 특정 산업군(게임,스타트업 등)들에서 사용하는 것을 기반으로 MainStream으로 진출하는 중
Enterprise Application - ERP, CRM, BPM.... , 대개 Unix기반임

기존 시스템을 확장(스토리지 등) 2. 신규 서비스를 런칭 3. 기존 서비스를마이그레이션
Enterprise가 도입하면 좋은 부분???? 메가마트라는 온라인 쇼핑몰 - 농심 계열 대형 마트점 ---> Pain Point 특정 이벤트, 행사 시 동접 증가로 시스템 폭주 -> 클라우드로 이관 결정

TCP 산정(총소유비용) ----> 클라우드에서 약 45% 절감.
도쿄 AWS와 그룹 IDC간에 VPN을 통해 데이터 등을 동기화(네트워크 레이턴시에 대한 우려가 있었으나.. VPN(이중화)만으로 가능했음.)

1개월만으로 구축 완료 -> 빠른 Delivery 가 가능해짐, 2/25 오픈, 3/11~14 블랙데이 기간에도 안정적으로 서비스 되었음.
--- 방문자 49%, 페이지뷰 16% 증가, 응답시간 빨라짐

결과 - 매출 58% 증가, 6년 TCO기준으로 45%정도 비용 절감함
----> 자원을 탄력적으로 활용할 수 있는 부분이 매우 쉽고, 간편하다, 글로벌 확장에 좀 더 유연


Enteprise Application 마이그레이션 고려사항
---

TCO검토- 비용 절감이 다가 아님. 확장성, 비용, 고가용성, 유연성.
마이그레이션 방식 - folklift, Optimize, 절충형(embrace) --> 환경을 고려하여 최적의 방식을 선택
라이선스 검토 - BYOL-기존라이센스 재활용, Pay-as-you-go-유틸리티 스타일로 사용
-----> 벤더와 정책 협의 필요

시스템 Sizing - 객관적인 자료 확보 힘듦 ---> 클라우드 상에서는 예측 부담 감소(Scale out가능하기 때문에) - 실제 트래픽 증가에 효과적인 정책 수립

Hybrid 전략 검토, 전용선 검토, VPN이중화 검토

HA - 절충된 선에서 확보

On-Promise 상의 Enterprise Application의 cloud 이전


AWS를 통한 클라우드 보안 이해하기
---

클라우드에서의 보안 - AWS 상에서 어떻게 보안을 강화할 수 있는지에 대해
---> 클라우드는 기존과 조금 다르게 가야 하지 않는가???
Multi Level Security
클라우드 상의 어플리케이션은 계속 형상이나 구조가 변경된다.... 시시각각 변하는 형태

AWS Config
Cloudwatch -> 모니터링

AWS환경에서의 암호화 옵션
---> AWS Key Management Service - 암호화 키의 생성, 관리, 사용, 삭제를 관리해줌
---> 아예 EBS생성시 암호화 하여 생성할 수 있음
S3에 대한 암호화도 가능 -------> 연계되어 있는 서비스에 녹아들어 있음.
(나스닥에서 분석하고 redshift에서 암호화하는 부분을 사용중)
