---
layout: post
title: Kubernetes Installation
date: 2019-12-24 17:06:00
tags:
- Tech
- Docker
- Kubernetes
---

Kubernetes Installation 방법

# Kubernetes

Goole에서 만든 컨테이너 오케스트레이션을 위한 플랫폼. 사실상 표준이 되어가고 있는 듯 함. 여러개의 노드(서버) 상에서 컨테이너를 자동으로 생성,관리해준다. 구글에서 내부적으로 오랜기간 사용해오던 환경을 오픈소스화 하였다고 함.
기본적으로 master-nodes 구성으로 master는 관리 및 제어작업만 해 줌, multi master도 가능하다 여기서는 sinle master,multi node로 설치해 볼 예정

- 참고 : https://www.linuxtechi.com/install-configure-kubernetes-ubuntu-18-04-ubuntu-18-10/
- 참고 : https://linuxconfig.org/how-to-install-kubernetes-on-ubuntu-18-04-bionic-beaver-linux
- 참고 : https://hiseon.me/linux/ubuntu/ubuntu-kubernetes-install/
- 설치환경 : 개발용 노트북, vagrant에 의해 생성된 ubuntu18.04 linux vm 4대(ram은 master 4GB,node 2GB)

# Pre-Installation

vagrant 설정 - 단순히 cpu,mem정도만

    #Vagrantfile
    Vagrant.configure("2") do |config|

        config.vm.define "master" do |cfg|
            cfg.vm.box = "ubuntu/bionic64"
            cfg.vm.network "private_network", ip: "192.168.100.100"
            cfg.vm.host_name = "master"
            cfg.vm.provider "virtualbox" do |vb|
            vb.memory = "4096"
            end
        end

        config.vm.define "node1" do |cfg|
            cfg.vm.box = "ubuntu/bionic64"
            cfg.vm.network "private_network", ip: "192.168.100.101"
            cfg.vm.host_name = "node1"
            cfg.vm.provider "virtualbox" do |vb|
            vb.memory = "2048"
            end
        end

        config.vm.define "node2" do |cfg|
            cfg.vm.box = "ubuntu/bionic64"
            cfg.vm.network "private_network", ip: "192.168.100.102"
            cfg.vm.host_name = "node2"
            cfg.vm.provider "virtualbox" do |vb|
            vb.memory = "2048"
            end
        end

        config.vm.define "node3" do |cfg|
            cfg.vm.box = "ubuntu/bionic64"
            cfg.vm.network "private_network", ip: "192.168.100.103"
            cfg.vm.host_name = "node3"
            cfg.vm.provider "virtualbox" do |vb|
            vb.memory = "2048"
            end
        end

    end



# Installation

각각의 노드 별로 작업해 주어야 함.

## docker install

    https://docs.docker.com/install/linux/docker-ce/ubuntu/

    sudo apt-get update

    sudo apt-get install \
        apt-transport-https \
        ca-certificates \
        curl \
        gnupg-agent \
        software-properties-common

    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -


    sudo add-apt-repository \
    "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
    $(lsb_release -cs) \
    stable"

    sudo apt-get update

    #sudo apt-get install docker-ce docker-ce-cli containerd.io
    #위와 같이 최신 말고 특정 버전으로 설치
    apt-cache madison docker-ce 으로 버전 확인 후 해당 버전을 아래쪽에 입력
    sudo apt-get install docker-ce=5:18.09.9~3-0~ubuntu-bionic docker-ce-cli=5:18.09.9~3-0~ubuntu-bionic containerd.io

    #test
    sudo docker run hello-world


## Kubernetes Install

host파일을 다음과 같이 수정

    192.168.100.100 master
    192.168.100.101 node1
    192.168.100.102 node2
    192.168.100.103 node3

키 설정

    curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add

    #-> bionic이 들어가면 아직 오류가 발생하여 xenial로 설치
    sudo add-apt-repository "deb https://apt.kubernetes.io/ kubernetes-xenial main"
    sudo apt install kubeadm 

    # 모든 노드에서 swap을 비활성화
    sudo swapoff -a

위의 내용까지는 모든 노드에서 작업

    master에서 k8s init
    sudo kubeadm init --pod-network-cidr=172.168.10.0/24

위와 같이 해도 아무런 응답이 없음.(문제 발생)


## vagrant에서 연결 오류시 해결방안

vagrant 상에서는 vm을 실행하여 설정시 조금 다르게 설치해야 함. 그렇지 않을 경우 물리적으로 연결이 된다고 해도, kubectl 상에서 node연결이 안됨(오류도 보이지 않고 응답이 없음)

참고 - https://medium.com/@joatmon08/playing-with-kubeadm-in-vagrant-machines-36598b5e8408

    # 아래와 같이 init시 network설정을 조그 다르게 해 줘야 함.
    kubeadm init --apiserver-advertise-address 192.168.100.100 --pod-network-cidr 10.1.0.0/16
    export KUBECONFIG=/etc/kubernetes/admin.conf

    # dns 라우팅 설정
    kubectl apply -f https://raw.githubusercontent.com/cloudnativelabs/kube-router/master/daemonset/kubeadm-kuberouter.yaml

    #각 node에서는 아래와 같이 실행함(token과 hash값은 달라짐)
    kubeadm join 192.168.100.100:6443 --token 8bfbxq.aszb963vy1lm3xa3 \
    --discovery-token-ca-cert-hash sha256:62e7f0d23f4b6246648f0e4ec902dac89acce7f269a0d49b9f655b6a9ba52252

    #리셋하기-경우에따라 재설정해야 할 경우
    kubeadm reset

시스템 재기동 후에도 설정을 적용하기 위해서는 .bashrc 등에 다음 문장을 추가해 놓음
export KUBECONFIG=/etc/kubernetes/admin.conf

## simple smoke test 

- 간단한 테스트

    $ kubectl run ska-nginx --image nginx --port=80
    deployment "ska-nginx" created

    $ kubectl expose deployment ska-nginx --type=NodePort
    service "ska-nginx" exposed

    $ kubectl get deployment
    NAME              DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
    ska-nginx   1         1         1            1           13s

    $ kubectl get services
    NAME              TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
    ska-nginx   NodePort    10.108.95.167   <none>        80:30278/TCP   3m
    kubernetes        ClusterIP   10.96.0.1       <none>        443/TCP        7m

    $ curl http://위의NodePort쪽IP주소     (서버확인)
