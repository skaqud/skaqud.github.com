---
layout: post
title: AWS S3 파일서버로 사용하기
date: 2017-03-14 11:48:00
tags:
- AWS
- Cloud
- Tech
---

S3는 AWS console을 통해서 접근 가능하며, 그 이외의 경우 별도의 클라이언트 프로그램들을 사용하거나, IAM에서 계정을 생성하고 권한을 부여하여 accesskey,secretkey를 생성하여 python, java등의 언어로 접근할 수도 있다. CDN처럼 활용할 수도 있고, 인스턴스에 마운트 한 뒤 NAS처럼 사용할 수도 있다.(속도 때문에 권장하지 않을 수도..) 현존하는 가장(?인지는 모르겠음) 가성비가 좋은 편인 파일 관리서비스라고 보면 될 것 같다.

참고 - [AWS S3FS vs EBS Performance](http://blog.leedoing.com/39)

## 사용하기 위한 절차

- S3에서 bucket을 생성
- 권한 부여(IAM) : IAM에 접속하여 Group과 User를 생성한 뒤 Access key ID, Secret access key를 확인(Role로도 가능하나 User가 더 간단함)
  - 권한은 가급적이면 다운로드만 할 경우 Read만 부여(AmazonS3ReadOnlyAccess)
- 해당 Access key와 Secret key를 통해 서버에 연결하고, 파일을 올리거나, 내려받는 작업을 수행함

## 연결방법

### GUI 클라이언트로 연결

다음과 같은 다양한 GUI 클라이언트가 있음, 권한만 있다면, 일단 GUI FTP 클라이언트 처럼 사용 가능

- CloudBerry Explorer: http://www.cloudberrylab.com/free-amazon-s3-explorer-cloudfront-IAM.aspx (FTP Client와 유사)
- Bucket Explorer: http://www.bucketexplorer.com

### CLI 클라이언트로 연결

다음과 같이 설치 및 설정.(ubuntu 14,16에서)

    # install
    $ apt-get -y install awscli

설정시 AccessKey, SecretKey, resion정보는 꼭 필요하다. 다른 형태로도 입력 가능함.    

    # configure - 해당 값을 입력
    $ aws aws configure
    AWS Access Key ID [None]: xxxx
    AWS Secret Access Key [None]: xxxxx 
    Default region name [None]: ap-northeast-1
    Default output format [None]:

    # s3에서 파일을 로컬로 copy
    $ aws s3 cp s3://my-bucket-for-fileserver/petclinic.war ./petclinic.war

이외 많은 작업들을 할 수 있다. 업데이트 정도는 sync로 처리해도 될 듯 하다.

### Saltstack으로 연결

pillar에 다음 내용을 추가한 뒤

    s3.keyid: AKIAIW2OXXXXXXXXXXXXXXX
    s3.key: avSB4Ks2LxxxxxxxxxxxxxxxxxxxxxxxxxxxxC
    s3.role_arn: arn:aws:iam::1145xxxxxx:user/s3xxxx
    s3.service_url: s3-ap-northeast-1.amazonaws.com
    s3.verify_ssl: False
    s3.location: ap-northeast-1

다음과 같이 받을 수 있다고 함

    s3test:
      file.managed:
        - name: /root/petclinic.war
        - source: s3://my-bucket-for-fileserver/petclinic.war
        - skip_verify: True
        #    - source_hash: sha512=해쉬값

위의 방법이 문제가 있어 salt가 awscli툴을 호출하여 파일을 다운로드 하도록 처리

pillar에 추가

    s3.keyid: xxxx
    s3.key: xxxx
    s3.region: ap-northeast-1
    s3.bucket: my-bucket-for-fileserver

state에 추가(인증 정보를 env에 set)

    install_awscli:
      pkg:
        - installed
        - name: awscli

    make_s3_credential1:
       environ.setenv:
         - name: AWS_SECRET_ACCESS_KEY
         - value: {{ s3_key }}
         - update_minion: True

    make_s3_credential2:
       environ.setenv:
         - name: AWS_ACCESS_KEY_ID
         - value: {{ s3_keyid }}
         - update_minion: True

    s3_filedownload:
      cmd.run:
        - name: aws s3 cp s3://{{ s3_bucket }}/{{ s3_filename }} ./{{ s3_filename }} --region={{ s3_region }}

### Java로 연결

다음 문서를 참고하면 될 것 같습니다.

[AWS SDK for Java를 사용하여 객체 업로드](https://docs.aws.amazon.com/ko_kr/AmazonS3/latest/dev/UploadObjSingleOpJava.html)


## 참고

- [브라우져에서 아마존 S3 파일 업로드](http://blog.jeonghwan.net/브라우져에서-아마존-s3-파일-업로드/)
- [AWS S3 Client Tool 및 CLI 정리](http://blog.leedoing.com/21)