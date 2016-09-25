---
layout: post
title:  "Docker Ochestration 도구 및 방법들"
date:   2016-09-21 01:47:00
tags:
- Tech
- Docker
---

컨테이너 Ochestration 서비스의 동향?,사용과 관련하여...

아직도 춘추전국시대 느낌인 듯.

참고 - [Docker Swarm vs. Kubernetes](https://outofbedlam.github.io/docker/2016/03/12/DockerSwarm-vs-Kubernetes/)

참고2 - [Container Trends: Orchestration Tool Choices by User Type – A Dataset from Bitnami] (http://redmonk.com/fryan/2016/08/15/container-trends-orchestration-tool-choices-by-user-type-a-dataset-from-bitnami/)

처음에는 Kubernetes와 Swarm 뿐이었고, Kubernetes가 우세했으나, 지금 와선 잘 모르겠음.

이외에 Nomad, Mesos 등 처럼 분산 클러스터 관리 플랫폼에서 docker를 사용할 수 있도록 한 경우도 있고, Rancher와 tutum이라는 것도 있는 것 같다.([Battle of the Docker Orchestrators: Rancher vs Tutum](https://www.brianchristner.io/battle-of-the-docker-orchestrators-tutum-vs-rancher/))


Docker 기반 관리도구 비교

- Docker Datacenter- 유료
- Docker Cloud-클라우드 기반 SaaS솔루션, 유료
- Docker UCP - Standalone으로 쓰면 무료

[Q. Docker Cloud vs Docker UCP vs Docker Datacenter?](https://forums.docker.com/t/docker-cloud-vs-docker-ucp-vs-docker-datacenter/7305/2)

- Docker Datacenter - CAAS (containers as a service) solution. Can be installed on your own VMs, inside your own public or private cloud or on traditional custom infrastructure. Comprised of the two solutions (Docker UCP and Docker Trusted Registry). ($125 / month / engine)

- Docker Cloud - SAAS solution. Create a docker cloud account and run containers on your own VMSs, but the infrastructure is hosted and operated by Docker. This is a managed solution for container deployments. Comparable to Docker UCP, but with a different target customer. Integrates well with Docker Hub. Focused on simplifying the CI pipeline for containers. ($15 / month / engine)

- Docker UCP - The management component of Docker Datacenter. Adds a UI and security features on top of Docker Swarm. This is more of a complete solution that can handle who can run which containers where on top of your infrastructure. Comes with some monitoring capabilities as well. You probably won't be using Docker UCP standalone, but instead it would come with your Docker Datacenter subscription. (See docker Datacenter for pricing).
