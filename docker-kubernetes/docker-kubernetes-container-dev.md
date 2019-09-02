# 도커/쿠버네티스를 활용한 컨테이너 개발 실전 입문

# 2장 도커 컨테이너 배포

## 1. 컨테이너로 애플리케이션 실행하기

### 도커 이미지 빌드하기
```
docker image build -t 이미지명[:태그명 ]'Dockerfile의 경로'
```
- `-t` 옵션으로 이미지명을 지정.
- 태그명도 지정 가능. 생략 시에는 latest 자동으로 붙음.
- '네임스페이스/이미지명' 으로 사용자 네임스페이스 추가 가능.

- CMD 인자표기는 배열 방식을 권함.
```
CMD ["실행 파일", "인자1", "인자2"]
```

### 도커 컨테이너 실행

- `-d` 데몬으로 실행.

```
$ docker container run -d example/echo:latest
```

#### 포트포워딩

- 컨테이너 포트는 외부에서 접근이 불가능함.
- 포트포워딩으로 호스트 머신의 포트를 컨테이너 포트와 연결해줘야함.
```
$ docker container run -d -p 9000:8080 example/echo:latest
```

## 2. 도커 이미지 다루기

### docker image build

#### --pull 옵션

```
$ docker image build --pull=true -t example/echo:latest .
```
- 로컬에 베이스 이미지 캐시가 있다면 변경된 부분만 반영해서 빌드함.
- 이미지 빌드 시 로컬에 받아놓은 이미지가 아닌 레지스트리에서 최신 베이스 이미지를 받고 싶다면 필요함.

### docker search - 이미지 검색

```
docker search [options] 검색키워드

$ docker search --limit 5 mysql
```

### docker image pull - 이미지 내려받기

```
docker image pull [options] 레포지토리명[:태그명]

$ docker image pull jenkins:latest
```

### docker image ls - 보유한 도커 이미지 목록 보기

```
docker image ls
```

- 호스트 운영체제에 있는 이미지 목록을 보여줌. pull 받은 것 & build 한 것.

### docker image tag - 이미지에 태그 붙이기

- 태그 하나에 연결될 수 있는 이미지는 하나 뿐.
- 애플리케이션을 수정하고 빌드하면 latest였던 태그가 <none>이 됨.

```
docker image tag 기반이미지명[:태그] 새이미지명[:태그]

$ docker image tag example/echo:latest example/echo:0.1.0
```

### docker image push - 이미지를 외부에 공개하기

```
docker image push [options] 레포지토리명[:태그]
```

## 3. 도커 컨테이너 다루기

### 도커 컨테이너 생애주기

- 각 컨테이너는 같은 이미지로 생성했다고 하더라도 별개의 상태를 갖음.
- 실행 중, 정지, 파기
- 정지 상태의 컨테이너는 다시 실행 가능.
- 파기한 컨테이너는 다시 실행 불가능.

### docker container run - 컨테이너 생성 및 실행

```
docker container run [options] 이미지명[:태그] [명령] [명령인자...]
docker container run [options] 이미지ID [명령] [명령인자...]
```

#### 컨테이너 이름 붙이기

```
docker container run --name [컨테이너명] [이미지명][:태그]

$ docker container run -t -d --name gihyo-echo example/echo:latest
```

- 이름 붙인 컨테이너는 개발용으로는 자주 사용되지만 운영 환경에서는 거의 사용되지 않음.
- 같은 이름의 컨테이너를 새로 실행하려면 같은 이름을 갖는 기존의 컨테이너를 먼저 파기해야 하기 때문.
- 많은 수의 컨테이너를 계속 생성 및 실행하고, 정지시켰다가 파기시키는 과정을 반복하는 운영 환경에는 적합하지 않음.

### docker container ls - 도커 컨테이너 목록 보기

#### 컨테이너 ID만 추출하기

```
docker container ls -q
```

#### 컨테이너 목록 필터링하기

```
docker container ls --filter "필터명=값"

$ docker container ls --filter "name=echo1"
```

### docker container stop - 컨테이너 정지하기

```
docker container stop 컨테이너ID 또는 컨테이너명

$ docker container stop echo
```

### docker container restart - 컨테이너 재시작하기

```
docker container restart 컨테이너ID 또는 컨테이너명

$ docker container restart echo
```

### docker container rm - 컨테이너 파기하기

```
docker container rm 컨테이너ID 또는 컨테이너명
```

- 실행 중인 컨테이너는 삭제가 불가능.
- `rm -f`로 삭제해야함.
- run 옵션에 붙이면 정지됨과 동시에 삭제됨.

### docker container logs - 표준 출력 연결하기

```
docker container logs [options] 컨테이너ID 또는 컨테이너명
```

### docker container exec - 실행 중인 컨테이너에서 명령 실행하기

```
docker container exec [options] 컨테이너ID 또는 컨테이너명 컨테이너에서 실행할 명령

$ docker container run -t -d --name echo --rm example/echo:latest
$ docker container exec echo pwd
/go

$ docker container exec -it echo sh
pwd
/go
```

- 컨테이너 내부 상태를 확인하거나 디버깅하는 용도로 활용 가능.
- 컨테이너 안에 든 파일을 수정하는 건 부작용이 생길 수 있으므로 운영 환경에서는 절대 하면 안 됨.

### docker container cp - 파일 복사하기

```
docker container cp [options] 컨테이너ID 또는 컨테이너명:원본파일 대상파일

# 컨테이너 -> 호스트
$ docker container cp echo:/echo/main.go .

# 호스트 -> 컨테이너
$ docker container cp dummy.txt echo:/tmp
$ docker container exec echo ls /tmp | grep dummy
dummy.txt
```

- 실행 중인 컨테이너와 파일을 주고 받는 방법.
- 정지 상태의 컨테이너에 대해서도 실행할 수 있음.

## 4. 운영 관리를 위한 명령

### prune - 컨테이너 및 이미지 파기

```
# 실행 중이지 않은 컨테이너를 모두 삭제.
docker container prune [options]

# 태그가 붙지 않은 모든 이미지 삭제.
docker image prune [options]

# 사용하지 않는 이미지, 컨테이너, 볼륨, 네트워크 등 모든 리소스 일괄 삭제.
docker system prune
```

### docker container stats - 사용 현황 확인하기

```
docker container stats [options] [대상_컨테이너ID...]
```

## 5. 도커 컴포즈로 여러 컨테이너 실행하기

### docker-compose 명령으로 컨테이너 실행하기

- 여러 컨테이너의 실행을 한 번에 관리할 수 있게 해줌.

```
# docker-compose.yml
version: "3"
services:
  echo:
    image: example/echo:latest
    ports:
      - 9000:8080

$ docker-compose version
$ docker-compose up -d

# 새로 이미지를 강제로 다시 빌드
$ docker-compose up -d --build
```

# 3장 컨테이너 실전 구축 및 배포

## 1. 애플리케이션과 시스템 내 단일 컨테이너의 적정 비중

### 컨테이너 1개 = 프로세스 1개?

- cron과 api를 별도의 컨테이너로 구성하는 것 보다 1개의 컨테이너로 구성하는게 훨씬 깔끔.
- 컨테이너 1개 = 프로세스 1개를 고수할 필요가 없음.

#### 자식 프로세스를 너무 의식하지 말 것.

- Apache, Nginx는 여러 프로세스가 동작하게 되는데 컨테이너 1개 = 프로세스 1개를 고수하는 건 비현실적.

### 컨테이너 1개에 하나의 관심사

- 컨테이너 하나가 한 가지 역할이나 문제 영역(도메인)에만 집중해야 한다는 의미.
- 각 애플리케이션과 컨테이너가 전체 시스템에서 차지해야 하는 적정 비중을 고려하면 "각 컨테이너가 맡을 역할을 적절히 나누고, 그 역할에 따라 배치한 컨테이너를 복제해도 전체 구조에서 부작용이 일어나지 않는가?"를 따져가며 시스템을 설계해야함.

## 2. 컨테이너의 이식성

### 커널 및 아키텍처의 차이

- 호스트형 가상화 기술은 하드웨어를 연산으로 에뮬레이션하지만 컨테이너형 가상화 기술은 호스트 운영체제와 커널 리소르를 공유함.
- 이는 도커 컨테이너를 실행하려면 호스트가 특정 CPU 아키텍쳐 혹은 운영 체제를 사용해야 한다는 의미.
- x86_64 아키텍쳐에서 실행하는 것을 전제로 만들어진 이미지는 다른 아키텍쳐에서 동작이 보장되진 않음.

### 라이브러리와 동적 링크 문제

- 정적 링크: 애플리케이션에 사용된 라이브러리를 내부에 포함하는 형태이므로 애플리케이션의 크기가 비대해지는 경향이 있지만 이식성은 뛰어남.
- 동적 링크: 애플리케이션을 실행할 때 라이브러리가 링크되므로 애플리케이션 크기는 작아지지만 애플리케이션을 실행할 호스트에서 라이브러리를 갖춰야 하는 문제가 생김.
- 동적 링크는 예를 들어, CI와 컨테이너에서 사용하는 표준 C 라이브러리가 서로 다르면 CI에서 빌드해 복사해 온 애플리케이션은 컨테이너에서 동작하지 않음.

## 3. 도커 친화적인 애플리케이션

### 환경 변수 활용

- 컨테이너 형태로 실행되는 애플리케이션의 동작을 제어하는 4가지 방법
  - 실행 시 인자
  - 설정 파일
  - 애플리케이션 동작을 환경 변수로 제어(추천)
  - 설정 파일에 환경 변수를 포함

#### 실행 시 인자를 사용

- 외부에서 값을 주입받을 수 있다는 장점은 있지만 인자를 내부 변수로 매핑해주는 처리가 복잡해지고 내용 관리가 어려워질 수 있음.

#### 설정 파일 사용

- 특정 환경에 대한 설정 파일을 도커 이미지에 포함시키면 이식성을 해치게 됨.
- 설정 파일을 추가할 때마다 이미지를 새로 빌드.

#### 애플리케이션 동작을 환경 변수로 제어

- 환경 변수 값만 바꾼 후 컨테이너를 다시 시작하면 됨.
- 환경 변수는 애플리케이션과 별도의 레포지토리를 통해 관리하는 것이 일반적.

#### 설정 파일에 환경 변수를 포함

- 설정파일에 환경 변수를 포함할 수 있다면 환경 변수의 장점과 설정 파일의 장점을 모두 취할 수 있음.
- Spring, Node.js 등...
- 모든 프레임워크가 이런 기능을 지원하는 것은 아니므로 컨테이너 친화적인 애플리케이션을 만들기에 유리한 프레임워크인지를 평가할 때 지표로 사용하면 좋을 것.

## 4. 퍼시스턴스 데이터를 다루는 방법

- 컨테이너 실행 중에 작성 혹은 수정된 파일은 호스트 쪽 파일 시스템에 마운트되지 않는 한 컨테이너가 파기 될 때 호스트에서 함께 삭제.
- 애플리케이션이 stateful하다면 파기된 컨테이너를 완전히 동일하게 재현하기는 어려움.
- 이전 컨테이너에서 사용하던 파일 및 디렉터리를 그대로 이어받아 사용하기 위한 것이 data volume.

### 데이터 볼륨

- 컨테이너 안의 디렉터리를 디스크에 퍼시스턴스 데이터로 남기기 위함.
- 호스트 쪽 특정 디렉터리에 의존성을 갖게 됨.
- 호스트 쪽 데이터 볼륨을 잘못 다루면 애플리케이션에 부정적 영향을 미칠 수 있긴 함.

```
docker container run [options] -v 호스트_디렉터리:컨테이너_디렉터리 레포지토리명[:태그] [명령] [명령인자]

$ docker container run -v ${PWD}:/workspace gihyodocker/imagemagick:latest convert -size 100x100 xc:#000000 /workspace/gihyo.jpg
```

### 데이터 볼륨 컨테이너

- 컨테이너 간에 디렉터리를 공유하는 방법.
- 데이터를 저장하는 것만이 목적인 컨테이너.
- 데이터 볼륨은 호스트 머신의 `/var/lib/docker/volumes/` 아래에 위치함.
- 컨테이너가 호스트 볼륨 디렉터리를 알 필요가 없음.
- 애플리케이션과 데이터의 결합이 느슨해짐.

