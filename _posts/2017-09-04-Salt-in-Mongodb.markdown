---
layout: post
title: Salt와 Mongodb 연결방법
date: 2017-09-04 11:04:00
tags:
- Tech
- Devops
- Saltstack
---

salt-master의 Top파일 부분을 DB로 대체하여, 인프라, SW설치와 관련된 부분을 DB화 하는 방법

대부분 [참고: Howto move Saltstack tops and pillar contents to MongoDB](https://rene.jochums.at/howto-move-saltstack-tops-and-pillar-contents-to-mongodb/)
의 블로그 내용을 그대로 따라 해 보고 한글로 옮긴 것임.

[참고2: Salt, storing Pillar data in MongoDB](http://www.tmartin.io/articles/2014/salt-pillar-mongodb/)


# salt-mongo 연계 테스트

우선은 연동을 위한 mongodb 설치 - [참고:Install MongoDB Community Edition on Ubuntu](https://docs.mongodb.com/master/tutorial/install-mongodb-on-ubuntu/)

    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6
    echo "deb [ arch=amd64 ] http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list

    sudo apt-get update
    sudo apt-get install -y mongodb-org
    sudo service mongod start

클라이언트로 접속 및 확인
mongo localhost

## mongo 설정

admin 계정 생성

    use admin;
    db.createUser({ user: "test",
      pwd: "test",
      roles: [
        { role: "clusterAdmin", db: "admin" },
        { role: "userAdminAnyDatabase", db: "admin" },
        { role: "readWriteAnyDatabase", db: "admin" },
        { role: "dbAdminAnyDatabase", db: "admin" },
      ]
    })
    quit();

다음과 같이 로그인 되는지 확인
mongo -u test --authenticationDatabase admin -p

salt 연결 계정 생성

    use saltstack;
    db.createUser({ user: "saltmaster",
      pwd: "saltmaster",
      roles: [
        { role: "readWrite", db: "saltstack" },
      ]
    })
    quit();

삭제시
db.removeUser(saltmaster)

sample salt tops 입력

    {
        "_id" : "local.test.com",
        "states" : [
            "roles.base.server",
            "roles.base.lxc"
        ],
        "environment" : "test"
    }

salt-master의 mongodb 설정 추가 및 수정

    master_tops:
      mongo:
        id_field: _id
        collection: salt_tops
    ext_pillar:
      - mongo: {collection: salt_pillar}
        ext_pillar_first: true

    #####   mongodb connection settings  #####
    ##########################################
    mongo.db: saltstack
    mongo.indexes: true
    mongo.host: 192.168.1.1
    mongo.user: saltmaster
    mongo.password: saltmaster
    mongo.port: 27017

동작하지 않아서 아래와 같이 설치 후 다시 시도 
apt-get install python-pymongo python-pymongo-ext


## 기동시 인증 에러

root@ubuntu64:~# salt '*' pillar.items
vagrant-ubuntu-trusty-64:
    ----------
    _errors:
        - Failed to load ext_pillar mongo: command SON([('authenticate', 1), ('user', u'saltmaster'), ('nonce', u'2d411717531348f2'), ('key', u'8123827b48e827e1d35c506c71427079')]) failed: auth failed

위의 에러시 다음과 같이 권한 관련 옵션을 변경 후
    mongo
    use admin
    db.system.users.remove({})    <== removing all users
    db.system.version.remove({}) <== removing current version 
    db.system.version.insert({ "_id" : "authSchema", "currentVersion" : 3 })

계정을 다시 생성한 뒤 다시 실행해보기 

salt '*' pillar.items
salt '*' state.apply test

아래는 테스트용 코드임

    /srv/salt/test# cat init.sls
    touch /tmp/foo:
      cmd.run:
        - creates: /tmp/foo

 

## 결론

master의 top을 관리하려는 목적으로 cobbler, varstack, mongodb, reclass adapter 등의 선택이 있음.
reclass adapter는 LDAP, CMDB와 같은 Nodes Inventory sw와 연결하는 방법인듯(http://www.tmartin.io/articles/2014/nodes-inventory-using-reclass/)
mongo가 가장 보편적인 듯 합니다.

다른 방법으로 external pillar를 관리하려는 목적으로 mongo, mysql, postgresql, http_json등 다양한 방법이 있음
postgres는 json 자체를 datatype으로 입출력 할 수 있어, 해당 DB에 json으로 넣은 뒤 작업 하면 어떨까 싶음 

