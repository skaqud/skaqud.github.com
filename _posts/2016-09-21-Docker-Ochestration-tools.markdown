---
layout: post
title:  "Docker Ochestration 도구 및 방법들"
date:   2016-09-21 01:47:00
tags:
- Tech
- Docker
---

현재 작성중입니다.


Docker 기반 관리도구 비교에 관한 글.

- Docker Datacenter- 유료
- Docker Cloud-클라우드 기반 SaaS솔루션, 유료
- Docker UCP - Standalone으로 쓰면 무료

[Q. Docker Cloud vs Docker UCP vs Docker Datacenter?](https://forums.docker.com/t/docker-cloud-vs-docker-ucp-vs-docker-datacenter/7305/2)

- Docker Datacenter - CAAS (containers as a service) solution. Can be installed on your own VMs, inside your own public or private cloud or on traditional custom infrastructure. Comprised of the two solutions (Docker UCP and Docker Trusted Registry). ($125 / month / engine)

- Docker Cloud - SAAS solution. Create a docker cloud account and run containers on your own VMSs, but the infrastructure is hosted and operated by Docker. This is a managed solution for container deployments. Comparable to Docker UCP, but with a different target customer. Integrates well with Docker Hub. Focused on simplifying the CI pipeline for containers. ($15 / month / engine)

- Docker UCP - The management component of Docker Datacenter. Adds a UI and security features on top of Docker Swarm. This is more of a complete solution that can handle who can run which containers where on top of your infrastructure. Comes with some monitoring capabilities as well. You probably won't be using Docker UCP standalone, but instead it would come with your Docker Datacenter subscription. (See docker Datacenter for pricing).








rancher 와 tutum??

https://www.brianchristner.io/battle-of-the-docker-orchestrators-tutum-vs-rancher/









swarm과 kubernetes 의 비교

https://outofbedlam.github.io/docker/2016/03/12/DockerSwarm-vs-Kubernetes/








도커 1.12 내장 오케스트레이션과 서비스 생성

https://outofbedlam.github.io/docker/2016/07/20/docker/

MapR, Mesos, Marathon, Docker, Spark

https://outofbedlam.github.io/2016/07/04/mesos/
