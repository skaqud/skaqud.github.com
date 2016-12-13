---
layout: post
title: HyperLedger-Blockchain
date: 2016-12-04 22:10:00
tags:
- Tech
---

개념을 잡기 위해 내용을 정리중(작성중)

# 하이퍼렛저

하이퍼렛저는 현존하는 다양한 오픈 소스 블록체인 접근 방식을 모두 통합하고자 하는 프로젝트다.

## 블록체인의 원리

블록체인은 재무 내역 항목 또는 거래 기록을 나타내는 데이터 구조다. 각 거래는 디지털 서명을 통해 진실성과 변조되지 않았음을 보장하므로 원장 자체와 원장 내의 거래는 높은 무결성을 갖는 것으로 간주된다.


그러나 블록체인의 핵심은 이러한 디지털 기록 항목이 구축 환경 또는 인프라 전반으로 분산된다는 데 있다. 인프라의 부가적인 노드와 계층들은 특정 시점에서 거래 상태에 관한 합의를 제공하는 역할을 한다. 모든 노드와 계층은 인증된 기존의 원장 사본을 분산된 형태로 갖는다.


원문보기:
http://www.itworld.co.kr/news/98843#csidxd99466d72f5fd039dda911e7e50e1b5

[Blockchain 에 대하여, 과거에서 미래까지](https://medium.com/@NipolNIpol/blockchain%EC%97%90-%EB%8C%80%ED%95%98%EC%97%AC-%EA%B3%BC%EA%B1%B0%EC%97%90%EC%84%9C-%EB%AF%B8%EB%9E%98%EA%B9%8C%EC%A7%80-a4917534327c#.cbxezbp0s)

Blockchain은 거대한 분산 장부(Ledger)입니다 --> HyperLedger이란말은 여기서 나온 듯.

모든 장부는 Block으로 표현되었고, 거래내역을 포함하고, 이전의 거래내역을 기반으로 새로운 Block을 생성하기 때문에 Chaining이 기본이 됩니다. 또한 모든 노드들의 잔고내역이 남기 때문에, "거래 가능한 잔고증명"도 가능하죠.

Blockchain에서는 해시 함수를 이용하여 다음과 같은 합의된 방법으로 Block을 검증합니다.
해시함수( “블럭 양식 버전” + “이전 블럭의 해시” + “이전 블럭 작성시간” + “새로운 블럭에 포함될 거래 갯수” + “블럭 생성자” +”난이도 계수” )
= “새로운 블럭의 해시”

블럭 생성자 -
난이도 계수 - Bitcoin의 “블럭 생성 난이도”는 해시의 시작을 0으로 하되, 0의 길이를 점점 증가시키는 것을 난이도를 상승시킨다

이와같은 연산능력을 사용하여 블럭을 증명하는 방식을 Proof of Work(PoW)라고 합니다.

[블록체인을 적용하기전 고려할 것들](https://brunch.co.kr/@jeffpaik/16)



현재 Linux Foundation홈페이지에 있는 프로젝트 목록

Fabric
The fabric is an implementation of blockchain technology that is intended as a foundation for developing blockchain applications or solutions. It offers a modular architecture allowing components, such as consensus and membership services, to be plug-and-play. It leverages container technology to host smart contracts called “chaincode” that comprise the application logic of the system.

Iroha
Iroha is a distributed ledger project that was designed to be simple and easy to incorporate into infrastructural projects requiring distributed ledger technology. Iroha features:
- simple construction
- modern, domain-driven C++ design
- emphasis on mobile application development
- new, chain-based Byzantine fault tolerant consensus algorithm, called Sumeragi

Sawtooth Lake
Sawtooth Lake is Intel’s modular blockchain suite. Distributed Ledger Technology has potential in many fields with use cases from IoT to Financials. This architecture recognizes the diversity of requirements across that spectrum. Sawtooth Lake supports both permissioned and permissionless deployments. It includes a novel consensus algorithm, Proof of Elapsed Time (PoET). PoET targets large distributed validator populations with minimal resource consumption


Satoshi Nakamoto - A Peer-to-Peer Electronic Cash System 원문
- 번역1:http://bitfun.kr/146
- 번역2:http://gommoo.blogspot.kr/2016/11/satoshi-nakamoto-bitcoin-peer-to-peer.html


하이퍼렛저 등 관련 참고 사이트
http://goodjoon.tistory.com/ - 이더리움 관련 교육내용

http://www.seunghwanhan.com/2016/07/hyperledger-project-hlp.html
- 소개

MOVING HYPERLEDGER TO THE LINUX FOUNDATION
https://digitalasset.com/press/open-sourcing-hyperledger-linux-foundation.html


http://www.seunghwanhan.com/2015/06/bitcoin-studies-analyses.html
비트코인 관련 자료정리

http://www.seunghwanhan.com/2016/07/introduction-to-bitcoin-blockchain.html
블록체인 개론
