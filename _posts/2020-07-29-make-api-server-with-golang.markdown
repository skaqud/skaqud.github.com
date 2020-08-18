---
layout: post
title:  "golang기반 RestAPI서버 작성"
date:   2020-07-29 10:43:00 +0900
tags:
- Docker
- golang
- wsl
---

* TOC
{:toc}

---


## 목적

Windows 기반의 개발환경에서 WSL을 통해 Linux기반의 golang API서버를 개발하여 Docker 상에서 서비스를 실행하는 방법에 대해 설명합니다. 


## 사전 준비사항

WSL이 설치된 Windows 기반 PC(정확하게는 WSL2버전)

### WSL 설정

기본적으로 여러개의 배포판을 설치할 수 있으나, 사용하는 도구에 따라 Default WSL을 정의할 수 있음.
(외부 도구에서는 WSL이라는 이름으로 디폴트쪽으로만 접근되는 경우가 많음, WSL1의 경우 일반적인 설치 방법으로 docker를 실행할 수 없어 WSL2 버전을 통해 실행)

WSL 상의 기본 ubuntu 배포판 버전 변경하기

https://winaero.com/blog/set-default-wsl-linux-distro-windows-10/

wsl --list --all 
로 아래와 같이 목록 확인 후

Ubuntu
Ubuntu-20.04

wsl --setdefault <DistributionName>
와 같이 변경

wsl --setdefault Ubuntu-20.04


### golang 개발환경 설정 on wsl 

golang은 최신 바이너리를 받아 별도 설치하고, 관련 디렉토리를 생성한 뒤, 로그인 쉘에 path를 설정해줌. 

    mkdir download
    cd download
    curl -L -O https://golang.org/dl/go1.14.6.linux-amd64.tar.gz
    sudo tar -C /usr/local -xzf go1.14.6.linux-amd64.tar.gz


    ls -ail /usr/local/go/bin/go

    mkdir ~/go
    mkdir ~/go/bin
    mkdir ~/go/pkg
    mkdir ~/go/src

    #path 설정 - .zshrc
    export GOPATH=$HOME/go
    export GOROOT=/usr/local/go
    export GOBIN=$HOME/go/bin
    export PATH=$PATH:.:$GOPATH/bin:/usr/local/go/bin

이후 파일 생성하고 테스트 


go를 wsl에 설치하고 설정하기 - [다음 내용](https://medium.com/@betakuang/setup-go-development-environment-with-vs-code-and-wsl-on-windows-62bd4625c6a7)을 참고하여


Remote-WSL, Go 플러그인을 먼저 설치 한 뒤 New Window로 창을 하나 띄우고 vscode 화면 좌측 하단의 >< 부분을 클릭하여 Remote-WSL:New Window 메뉴를 선택하여
테스트로 만든 golang 소스코드 위치($HOME/go/src/com.test/)를 엽니다.

Go플러그인에서 Install on WSL이라는 버튼이 활성화되어 설치한 뒤
golang 소스파일을 하나 열면 관련된 플러그인들이 함께 설치되기 시작합니다. 플러그인 갯수는 lint계열부터 test 계열까지 5~10개 정도이며, 각각 개별/일괄로 우측 하단에 설치 관련된 알람을 눌러 설치를 진행하면 됩니다.(사전에 wsl상의 인스턴스에서 go관련 path가 정상으로 설정되었다면 무리없이 진행할 수 있을듯 합니다.)

### go sample코드 생성

다음과 같은 샘플파일을 하나 만든 뒤, Run 까지 해보면서 관련 플러그인을 설치

    package main

    import (
      "fmt"
    )

    func main() {
      fmt.Println("This is main...")
    }

    func sum(a ...int) int {
      sum := 0
      for _, i := range a {
        sum += i
      }
      return sum
    }

정상적으로 동작하는지 확인하기 위해 다음과 같이 테스트코드를 만들어 테스트도 해 봅니다.

    package main

    import "testing"

    func Test_sum(t *testing.T) {
      v0 := sum(1, 2, 3, 4)
      if v0 != 10 {
        t.Fatal("1+2+3+4 == 10")
      }

      v1 := sum(6, 5)
      if v1 != 11 {
        t.Fatal("6+5 == 11")
      }

    }

테스트코드를 정상적으로 인식한 경우 vscode 상의 해당 테스트함수 바로 위에 run test 라고 메뉴가 보이며, 바로 테스트코드를 실행할 수 있습니다. 패키지 명 위에서는 해당 패키지의 테스트코드를 모두 수행하고 커버리지를 측정해 줍니다.

### Sample API 서버 만들기

go에서 애플리케이션 설계하기 - https://golangkorea.github.io/post/structuring-applications-in-go/

golang에서 일반적으로 웹 서비스를 만들기 위한 프레임워크를 사용하여 서비스를 만들어 보기로 합니다.
프레임워크 이름은 [gin](https://github.com/gin-gonic/gin) 이며 github에서 해당 사이트의 샘플 코드를 받은 뒤 아래와 같이 main.go 파일을 만들어 저장합니다.

    package main

    import (
      "net/http"

      "github.com/gin-gonic/gin"
    )

    func main() {
      router := gin.Default()

      // This handler will match /user/john but will not match /user/ or /user
      router.GET("/user/:name", func(c *gin.Context) {
        name := c.Param("name")
        c.String(http.StatusOK, "Hello %s", name)
      })

      // However, this one will match /user/john/ and also /user/john/send
      // If no other routers match /user/john, it will redirect to /user/john/
      router.GET("/user/:name/*action", func(c *gin.Context) {
        name := c.Param("name")
        action := c.Param("action")
        message := name + " is " + action
        c.String(http.StatusOK, message)
      })

      router.Run(":8080")

    }

실행을 위해서는 해당 프레임워크가 없어서 에러가 발생할 텐데, github의 document를 참고하여 아래와 같이 cli상에서 받을 수 있습니다.

    go get github.com/gin-gonic/gin

프레임워크를 받은 뒤 해당 코드를 실행시킨 다음 브라우저로 http://localhost:8080/user/1111 과 같이 호출하면 결과를 확인할 수 있습니다.
단순히 해당 user의 정보를 리턴하는 API이며, 좀 더 정식으로 하기 위해서는 뒤에 DB도 있어야 하고, 인증 등의 과정도 필요할 것 같습니다.

또한 go build main.go 와 같이 실행하면 나오는 실행파일을 docker로 만들어 Micro Service Architecture를 구성하는 서비스를 생성할 수 있습니다.

### WSL2에 Docker 설치

golang으로 빌드된 실행파일은 여타의 다른 라이브러리나, 설정이 필요 없이 단독으로 실행 가능한 파일이므로, Docker 이미지 안에 Copy후 단순히 서비스를 실행하는 것 만으로도 서비스 실행이 가능하며 크기나, 실행 방법 측면에서 다른 언어들에 비해 무척 가볍고 단순합니다.

앞의 API서비스를 빌드한 파일을 docker 이미지로 생성해 보겠습니다.

Dockerfile을 다음과 같이 만든 뒤(그 전에 wsl상의 OS에 docker는 설치되어 있어야 하겠네요) 생성된 main 파일을 해당 컨테이너에 copy하고 실행하도록 명령을 구성합니다.

- 참고 - [wsl2에서 docker 설치 및 사용법](https://www.44bits.io/ko/post/wsl2-install-and-basic-usage)

wsl1 에서는 docker를 사용하는 데 제약이 있었으나, wsl2에서는 가능한 것 같습니다. wsl1->2로 바꾸기 위해서는 

  - wsl 커널 업데이트를 위한 [파일](https://docs.microsoft.com/ko-kr/windows/wsl/wsl2-kernel)을 설치한다.
  - Windows에서 "윈도우 하이퍼바이저 플랫폼" 옵션을 활성화, 필요시 bios에서 가상화 옵션을 켜준다.
  - 가상 머신 플랫폼 옵션을 사용하도록 명령 실행하고 재기동하여 적용
    - 참고 - https://docs.microsoft.com/ko-kr/windows/wsl/install-win10#update-to-wsl-2
    - dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
  - 다음 명령을 실행하여 변환 ->  wsl --set-version Ubuntu-20.04 2

아래와 같이 WSL2에서는 일반적인 linux OS와 같이 Docker를 설치하는 것처럼 설치할 수 있으며

    $ sudo apt-get install \
        apt-transport-https \
        ca-certificates \
        curl \
        software-properties-common
    # (wsl1이라면 아래 부분이 진행이 안된다..)
    $ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add - 
    $ sudo apt-key fingerprint 0EBFCD88
    $ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
    $ sudo apt-get update
    $ sudo apt-get install docker-ce

설치 후 일반 유저에서 명령을 실행할 수 있도록 권한울 부여하고,

    $ sudo service docker start
    $ sudo usermod -aG docker $USER

이후 nginx 이미지를 다운로드 받아 실행해봅니다.

    $ docker pull nginx
    $ docker run -d -p 80:80 --name nginx nginx:latest

브라우저를 통해 http://localhost 를 호출하시면 정상적으로 설치되었음을 확인할 수 있습니다.

### Sample API Service Dockerize

golang 실행파일을 dockerize 하기 위해 Dockerfile을 만들고 컨테이너 빌드하는 과정이 필요합니다. 다음과 같이 Dockerfile을 생성합니다.

    FROM ubuntu                                                                         │
    COPY ./sample/api/api /sampleapi
    CMD ["/sampleapi"]    

다음과 같이 빌드 및 실행합니다.

    # build
    docker build -t sample.api:0.0.1 .
    # run
    docker run -d -p 8080:8080 --name sampleapi sample.api:0.0.1

이후 별도 실행했던 것과 동일하게 실행되는 것을 확인합니다.

~~컨테이너 이미지의 사이즈를 줄이기 위해 아래와 같이 alpine으로 변경하고, 추가적으로 코드를 입력합니다. ubuntu이미지에 비해 기본적인 도구들이 없거나, 부족하지만 매우 가볍습니다.~~

Ubuntu에서는 바이너리가 실행 되나, alpine에서는 not found 오류가 발생함, golang관련 설치가 필요한 건 아닌데, 일부 linux 라이브러리를 참조하는 듯 합니다. 향후 필요시 보완 예정

~~~
    FROM alpine:3.12.0
    COPY ./sample/api/api /usr/local/bin
    RUN chmod +x /usr/local/bin/api

    CMD ["api"]

https://johngrib.github.io/wiki/docker/ - 참고

    #빌드
    docker build -t sample.api:0.0.1 .
    # 빌드옵션
    GOOS=linux GOARCH=amd64 go build test.go
    # 실행
    docker run -d -p 8080:8080 --name sampleapi sample.api:0.0.1
~~~