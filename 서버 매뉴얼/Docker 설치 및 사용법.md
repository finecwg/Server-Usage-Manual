# Docker 설치 및 사용법

# 7.2.1. Docker 설치 및 사용법

이 문서에서는 Ubuntu 서버에 Docker를 설치하고 기본적인 사용법을 다루는 방법에 대해 자세히 설명합니다. Docker는 애플리케이션을 컨테이너화하여 일관된 환경에서 빠르게 배포하고 실행할 수 있도록 도와주는 오픈소스 플랫폼입니다. 이 문서에서는 Docker의 설치 과정, 기본 명령어, 이미지 관리, 컨테이너 실행 및 관리 등의 주제를 다룹니다.

## 7.2.1.1. Docker 설치

### 사전 요구 사항 확인

- 64비트 Ubuntu 운영 체제 (20.04 LTS 이상 권장)
- sudo 권한이 있는 사용자 계정

### 기존 Docker 설치 제거

- 이전 버전의 Docker 제거:
    
    ```bash
    sudo apt-get remove docker docker-engine docker.io containerd runc
    
    ```
    

이전에 설치된 Docker 버전이 있다면 충돌을 방지하기 위해 제거합니다.

### Docker 공식 저장소 추가

- 필요한 패키지 설치:
    
    ```bash
    sudo apt-get update
    sudo apt-get install ca-certificates curl gnupg lsb-release
    
    ```
    
- Docker의 공식 GPG 키 추가:
    
    ```bash
    curl -fsSL <https://download.docker.com/linux/ubuntu/gpg> | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
    
    ```
    
- Docker 안정 버전 저장소 추가:
    
    ```bash
    echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] <https://download.docker.com/linux/ubuntu> $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    
    ```
    

Docker 공식 저장소를 시스템에 추가하여 최신 버전의 Docker를 설치할 수 있도록 합니다.

### Docker 설치

- Docker 패키지 설치:
    
    ```bash
    sudo apt-get update
    sudo apt-get install docker-ce docker-ce-cli containerd.io
    
    ```
    

Docker 공식 저장소에서 Docker 엔진, CLI, containerd 패키지를 설치합니다.

### Docker 서비스 시작 및 자동 실행 설정

- Docker 서비스 시작:
    
    ```bash
    sudo systemctl start docker
    
    ```
    
- 부팅 시 Docker 자동 실행 설정:
    
    ```bash
    sudo systemctl enable docker
    
    ```
    

Docker 서비스를 시작하고, 시스템 부팅 시 자동으로 실행되도록 설정합니다.

### 설치 확인

- Docker 버전 확인:
    
    ```bash
    docker --version
    
    ```
    
- Docker 정보 확인:
    
    ```bash
    docker info
    
    ```
    

`docker --version` 명령어를 사용하여 Docker의 버전을 확인하고, `docker info` 명령어를 사용하여 Docker의 상세 정보를 확인합니다.

## 7.2.1.2. Docker 기본 명령어

### 이미지 관련 명령어

- 이미지 검색:
    
    ```bash
    docker search <이미지 이름>
    
    ```
    
- 이미지 다운로드:
    
    ```bash
    docker pull <이미지 이름>:<태그>
    
    ```
    
- 이미지 목록 확인:
    
    ```bash
    docker images
    
    ```
    
- 이미지 삭제:
    
    ```bash
    docker rmi <이미지 ID>
    
    ```
    

Docker Hub에서 이미지를 검색하고 다운로드할 수 있으며, 로컬에 저장된 이미지 목록을 확인하고 필요 없는 이미지를 삭제할 수 있습니다.

### 컨테이너 관련 명령어

- 컨테이너 실행:
    
    ```bash
    docker run -d --name <컨테이너 이름> <이미지 이름>
    
    ```
    
- 컨테이너 목록 확인:
    
    ```bash
    docker ps
    
    ```
    
- 실행 중인 컨테이너 접속:
    
    ```bash
    docker exec -it <컨테이너 ID> /bin/bash
    
    ```
    
- 컨테이너 중지:
    
    ```bash
    docker stop <컨테이너 ID>
    
    ```
    
- 컨테이너 삭제:
    
    ```bash
    docker rm <컨테이너 ID>
    
    ```
    

`docker run` 명령어를 사용하여 이미지를 기반으로 새로운 컨테이너를 실행할 수 있습니다. 실행 중인 컨테이너 목록을 확인하고, 컨테이너에 접속하여 내부에서 작업을 수행할 수 있습니다. 필요에 따라 컨테이너를 중지하거나 삭제할 수 있습니다.

## 7.2.1.3. Dockerfile을 사용한 이미지 생성

### Dockerfile 작성

- Dockerfile 예시:
    
    ```
    FROM ubuntu:20.04
    RUN apt-get update && apt-get install -y nginx
    EXPOSE 80
    CMD ["nginx", "-g", "daemon off;"]
    
    ```
    

Dockerfile은 Docker 이미지를 생성하기 위한 스크립트입니다. 기반 이미지를 지정하고, 필요한 패키지를 설치하며, 포트를 노출하고, 컨테이너 실행 시 실행될 명령어를 정의합니다.

### 이미지 빌드

- Dockerfile이 위치한 디렉토리에서 이미지 빌드:
    
    ```bash
    docker build -t <이미지 이름>:<태그> .
    
    ```
    

`docker build` 명령어를 사용하여 Dockerfile을 기반으로 새로운 이미지를 생성합니다. `-t` 옵션으로 이미지 이름과 태그를 지정하고, 마지막에 `.`을 사용하여 현재 디렉토리를 빌드 컨텍스트로 설정합니다.

### 생성된 이미지 실행

- 빌드된 이미지를 기반으로 컨테이너 실행:
    
    ```bash
    docker run -d --name <컨테이너 이름> -p 80:80 <이미지 이름>:<태그>
    
    ```
    

`docker run` 명령어를 사용하여 생성한 이미지를 기반으로 새로운 컨테이너를 실행합니다. `-d` 옵션으로 백그라운드에서 실행하고, `--name` 옵션으로 컨테이너 이름을 지정하며, `-p` 옵션으로 호스트와 컨테이너의 포트를 매핑합니다.

## 7.2.1.4. Docker Hub를 통한 이미지 공유

### Docker Hub 계정 생성

- Docker Hub 웹사이트([https://hub.docker.com](https://hub.docker.com/))에서 계정 생성

Docker Hub는 Docker 이미지를 공유하고 배포하기 위한 중앙 저장소입니다. Docker Hub에 계정을 생성하여 이미지를 공유할 수 있습니다.

### 이미지 태그 지정

- 이미지에 Docker Hub 사용자 이름을 포함한 태그 지정:
    
    ```bash
    docker tag <이미지 ID> <Docker Hub 사용자 이름>/<이미지 이름>:<태그>
    
    ```
    

Docker Hub에 이미지를 업로드하기 위해서는 이미지 이름에 Docker Hub 사용자 이름을 포함해야 합니다. `docker tag` 명령어를 사용하여 이미지에 적절한 태그를 지정합니다.

### Docker Hub에 이미지 업로드

- Docker Hub에 로그인:
    
    ```bash
    docker login
    
    ```
    
- 태그된 이미지 업로드:
    
    ```bash
    docker push <Docker Hub 사용자 이름>/<이미지 이름>:<태그>
    
    ```
    

`docker login` 명령어를 사용하여 Docker Hub에 로그인한 후, `docker push` 명령어를 사용하여 태그된 이미지를 Docker Hub에 업로드합니다.

이 문서에서는 Ubuntu 서버에 Docker를 설치하고 기본적인 사용법을 다루는 방법에 대해 자세히 설명하였습니다. Docker의 설치 과정, 기본 명령어, 이미지 관리, 컨테이너 실행 및 관리, Dockerfile을 사용한 이미지 생성, Docker Hub를 통한 이미지 공유 등의 주제를 포함하고 있습니다. 이 문서를 참조하여 Docker를 효과적으로 사용하고 관리할 수 있을 것입니다.

```

```