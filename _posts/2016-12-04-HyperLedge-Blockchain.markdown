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

# 블록체인

Hash를 이용해 거래 내역을 peer들끼리 공유하여 제3자의 개입 없이 peer들끼리 거래할 수 있는 수단을 제공하는 원천 기술. 금융권에는 새로운 대안으로 대두, 실생활에 여러가지 방법으로 활용될 수 있을 것 같음.

## 블록체인의 원리

### 소개

    Satoshi Nakamoto - A Peer-to-Peer Electronic Cash System 원문
    + 번역1:http://bitfun.kr/146
    + 번역2:http://gommoo.blogspot.kr/2016/11/satoshi-nakamoto-bitcoin-peer-to-peer.html

비트코인의 채굴 - mining

특정 해쉬값을 찾는 행위, 화폐의 발행은 10분당 한 번으로 마이닝하는 peer들로 인해 p2p 네트워크가 유지됨

http://www.slideshare.net/skimaza/ss-57356762
볼록체인은 분산되고 개발된 공통 Ledger(원장, 장부)를 관리하는 기술

Hashcash 문제를 푸는 노력(Proof of work) 에 대한 보상

총 통화의 개수는 한정되어 있고, 채굴되고 있는 보상은 줄어들고 있음.

원문보기:
http://www.itworld.co.kr/news/98843#csidxd99466d72f5fd039dda911e7e50e1b5

[Blockchain 에 대하여, 과거에서 미래까지](https://medium.com/@NipolNIpol/blockchain%EC%97%90-%EB%8C%80%ED%95%98%EC%97%AC-%EA%B3%BC%EA%B1%B0%EC%97%90%EC%84%9C-%EB%AF%B8%EB%9E%98%EA%B9%8C%EC%A7%80-a4917534327c#.cbxezbp0s)

Blockchain은 거대한 분산 장부(Ledger)입니다 --> HyperLedger이란말은 여기서 나온 듯. 분산, 독립, 개방

모든 장부는 Block으로 표현되었고, 거래내역을 포함하고, 이전의 거래내역을 기반으로 새로운 Block을 생성하기 때문에 Chaining이 기본이 됩니다. 또한 모든 노드들의 잔고내역이 남기 때문에, "거래 가능한 잔고증명"도 가능하죠.

[블록체인을 적용하기전 고려할 것들](https://brunch.co.kr/@jeffpaik/16)


### 원리

BlockChain이란 거래내역(Transaction)을 담고 있는 Block들의 연결, 이전의 유효한 블록을 기반으로 새로운 블록을 추가

해쉬함수는 SHA-2(SHA-256) 사용

다음 블록에 앞 블록의 Hash값을 가짐 + 현재 블록의 트랜잭션 요약(Payload의 해쉬) + nonce값

### 이더리움



### 하이퍼렛저 프로젝트(Hyperledger Project)

현재 Linux Foundation홈페이지에 있는 프로젝트 목록

Fabric

Iroha

Sawtooth Lake







## 참고

[블록체인 개요](http://www.slideshare.net/jeffpaik92/ss-52301202?next_slideshow=1)
[블록체인 업체 현황](http://www.slideshare.net/jeffpaik92/ss-53912943)


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
