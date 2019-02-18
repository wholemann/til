# Docker로 DevOps 시작하기

Last Edited: Feb 09, 2019 11:48 PM
Reference: 달랩 with 코딩의 신 아샬
Tags: DevOps,Docker

## DevOps?

- 안정성(신뢰) → Ops쪽에서 처리.
- 빠르게 변화 → Dev쪽에서 처리.
- 배포 → 장애, 서로 책임 전가, 기술부채.
- Agile 컨퍼런스에서 처음 발표됨.
- 자주 여러번 배포, 리드타임을 최소화.
- 결국은 개발조직과 운영조직의 통합.

## DevOps는 누가하나?

1. 소규모팀 → Architecture
2. 실제 환경과 유사 → Docker
3. 검증 → Unit Test 등 자동화된 테스트
4. 잠재적 문제 → Peer Review, 정적 분석 도구

## 장점?

- 변화가 두렵지 않아짐.
- 실험을 자주 할 수 있음. 망해도 됨. 실패 → 학습
- 배포를 좀 낮에 하자! 새벽에 말고!

## Server Ops

1. OS 버전
2. Package 1.3버전 저장소에 없네...?
3. application
- Snowflakes Server
- Pheonix Server(Re-born) → 피닉스 서버를 지원하기 위해 Docker를 씀
    - 가상화, 컨테이너

## Docker와 기존 VM의 차이점

- VM은 게스트OS 계층이 필요
- Docker는 필요없음
- 대신 HostOS랑 같은 OS를 써야함
- Docker는 LXC라는 리눅스 컨테이너 기술을 이용함

## Docker

- Image = CD-ROM
- 이미지 #1, 이미지#2 등 을 컨테이너에 바꿔서 교체해서 넣기
- hub.docker 깃허브 느낌. 원하는 이미지 받을 수 있음.
- docker pull golang:1.11.5

## Layer 개념

- OS, Package, Go, 우리 Application
- 계층을 계속 쌓아서 새로운 이미지를 만드는 개념

    docker run -it -v $PWD:/go/src/github.com/wholemann/my-new-app golang:1.11.5 bash
    
    docker image list
    
    docker container list
    docker container list -a
    
    docker run -it golang:1.11.5 bash
    
    # 마운트되어서 내 실제 로컬 디렉토리가 심볼릭 링크가 걸린다고 보면 됨.
    # 이미지에서 변하면 연결된 내 로컬에서도 변경됨.
    # 개발용으로 좋은 옵션. -v
    docker run -it -v $PWD:/go/src/github.com/wholemann/my-new-app golang:1.11.5 bash
    
    docker build -t my-new-app . # 마지막에 . 주의
    
    docker run -it my-new-app
    
    docker stop nginx
    docker start nginx

    FROM golang:1.11.4
    
    ENV APP_PATH=/go/src/github.com/ahastudio/my-new-app
    
    WORKDIR $APP_PATH
    
    # 얘네가 추가되면 아예 새로 이미지를 뜬다. re-born. 이게 피닉스 서버.
    # 중대한 변화가 있으면 새로 시작.
    #ADD package.json .
    #ADD package.lock .
    
    #RUN npm ci
    
    ADD . $APP_PATH
    
    CMD go run main.go

## dockerignore

- Dockerfile
- dockerignore
- git
- gitignore
- README.md

## 간단한 HTML 띄우기

[nginx - Docker Hub](https://hub.docker.com/_/nginx)

    mkdir -p ~/nginx/html
    echo "Welcome" > ~/nginx/html/index.html

    docker run -d --name nginx \
    	-p 8080:80 \
    	-v ~/nginx/html:/usr/share/nginx/html \
    	nginx
    docker logs -f nginx

http://localhost:8080/

## Continuous Integration

- Repository → Test → Build → Deploy
- Continuous Delivery
- Continuous Deployment

만약 배포하면서 nginx stop start를 하는데 10초 정도 걸리면 서비스 10초 정지임

## 무중단 배포 전략

1. Blue-Green 배포 전략
    - Blue : current version
    - Green : new version
2. 라우팅
    - ELB
    - L4
    - nginx upstream

1000대? Kubernetes

10000대? Mesos

    docker network create my-service
    docker network list
    docker run -it --network my-service --name app2 my-new-app bash