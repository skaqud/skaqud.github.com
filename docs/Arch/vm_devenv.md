# vm 기본환경 구성하기 #

vagrant 를 사용하여 vm을 올릴 경우 매번 환경이 초기화되어, 번거로운 부분들이 있는데, 이런 부분들에 대해 정리해두고 매번 삽질하지 않도록 하기 위함임

다음과 같은 내용들을 포함합니다.(ubuntu 14,16 버전 대상)

- shell설정 - zsh
- vim 설정 - .vim
- repository 설정
- git 설정
- 그밖에?


## apt-get(패키지관리자) 속도 개선 ##

repository를 daumkakao쪽 서버로 변경

    cd /etc/apt/
    cp sources.list sources.list.bak
    vi sources.list
    #vi 상에서 다음과 같이 입력하여 일괄 변경
    :%s/archive.ubuntu.com/ftp.daumkakao.com/g
    :%s/security.ubuntu.com/ftp.daumkakao.com/g
    # 변경정보 업데이트
    apt-get update

기본 어플리케이션 설치

git, wget, telnet, curl 등 설치
    
    apt-get install git wget telnet curl tmux

zsh 설치, 
    
    apt-get install zsh

기본 쉘 변경
    
    chsh -s /usr/bin/zsh

Oh my zsh 설치
curl -L https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh | sh

참고 - http://nolboo.kim/blog/2015/08/21/oh-my-zsh/

