# 이미지 관리, root dir 설정

# 7.2.2. 이미지 관리, root dir 설정

이 문서에서는 Docker 이미지의 효과적인 관리 방법과 Docker 데이터 디렉토리 변경 방법에 대해 자세히 설명합니다. Docker 이미지는 컨테이너를 생성하기 위한 템플릿으로, 이미지 관리는 Docker 사용에 있어 중요한 부분입니다. 또한 Docker의 기본 데이터 디렉토리를 변경하여 스토리지 관리를 최적화할 수 있습니다.

## 7.2.2.1. Docker 이미지 관리

### 이미지 검색 및 다운로드

- Docker Hub에서 이미지 검색:
    
    ```bash
    docker search <이미지 이름>
    
    ```
    
- 이미지 다운로드:
    
    ```bash
    docker pull <이미지 이름>:<태그>
    
    ```
    

`docker search` 명령어를 사용하여 Docker Hub에서 원하는 이미지를 검색할 수 있습니다. `docker pull` 명령어를 사용하여 이미지를 로컬 시스템으로 다운로드할 수 있습니다. 태그를 지정하지 않으면 기본적으로 `latest` 태그의 이미지가 다운로드됩니다.

### 이미지 목록 확인 및 상세 정보 조회

- 로컬에 저장된 이미지 목록 확인:
    
    ```bash
    docker images
    
    ```
    
- 이미지 상세 정보 조회:
    
    ```bash
    docker inspect <이미지 ID>
    
    ```
    

`docker images` 명령어를 사용하여 로컬 시스템에 저장된 이미지 목록을 확인할 수 있습니다. `docker inspect` 명령어를 사용하여 특정 이미지의 상세 정보, 구성, 메타데이터 등을 조회할 수 있습니다.

### 이미지 삭제

- 단일 이미지 삭제:
    
    ```bash
    docker rmi <이미지 ID>
    
    ```
    
- 복수 이미지 삭제:
    
    ```bash
    docker rmi <이미지 ID1> <이미지 ID2> ...
    
    ```
    
- 모든 이미지 삭제:
    
    ```bash
    docker rmi $(docker images -q)
    
    ```
    

`docker rmi` 명령어를 사용하여 불필요한 이미지를 삭제할 수 있습니다. 단일 이미지를 삭제할 때는 이미지 ID를 지정하고, 여러 이미지를 한 번에 삭제할 때는 이미지 ID를 공백으로 구분하여 나열합니다. 모든 이미지를 삭제할 때는 `docker images -q` 명령어로 이미지 ID 목록을 가져와 `docker rmi` 명령어에 전달합니다.

### 이미지 태그 지정

- 이미지에 태그 지정:
    
    ```bash
    docker tag <이미지 ID> <이미지 이름>:<태그>
    
    ```
    

`docker tag` 명령어를 사용하여 기존 이미지에 새로운 태그를 지정할 수 있습니다. 이미지 ID와 함께 새로운 이미지 이름과 태그를 지정합니다. 태그를 지정하면 동일한 이미지를 다른 버전이나 용도로 구분하여 관리할 수 있습니다.

## 7.2.2.2. Docker 데이터 디렉토리 변경

### Docker 데이터 디렉토리의 기본 위치

- 기본 Docker 데이터 디렉토리: `/var/lib/docker`

Docker는 기본적으로 `/var/lib/docker` 디렉토리에 이미지, 컨테이너, 볼륨 등의 데이터를 저장합니다.

### Docker 데이터 디렉토리 변경 방법

- Docker 서비스 중지:
    
    ```bash
    sudo systemctl stop docker
    
    ```
    
- 기존 Docker 데이터 디렉토리 백업 (선택 사항):
    
    ```bash
    sudo mv /var/lib/docker /var/lib/docker_backup
    
    ```
    
- 새로운 Docker 데이터 디렉토리 생성:
    
    ```bash
    sudo mkdir /new/path/docker
    
    ```
    
- Docker 데몬 설정 파일 수정:
    
    ```bash
    sudo vi /etc/docker/daemon.json
    
    ```
    
    ```json
    {
      "data-root": "/new/path/docker"
    }
    
    ```
    
- Docker 서비스 시작:
    
    ```bash
    sudo systemctl start docker
    
    ```
    

Docker 데이터 디렉토리를 변경하려면 먼저 Docker 서비스를 중지해야 합니다. 필요한 경우 기존 데이터 디렉토리를 백업한 후, 새로운 디렉토리를 생성합니다. `/etc/docker/daemon.json` 파일을 수정하여 `data-root` 옵션으로 새로운 데이터 디렉토리 경로를 지정합니다. 마지막으로 Docker 서비스를 다시 시작하여 변경 사항을 적용합니다.

### Docker 데이터 디렉토리 변경 시 주의 사항

- 기존 데이터 디렉토리의 백업 필요
- 새로운 데이터 디렉토리에 대한 적절한 권한 설정 확인
- 충분한 스토리지 공간 확보

Docker 데이터 디렉토리를 변경할 때는 기존 데이터를 백업하여 데이터 손실을 방지해야 합니다. 새로운 디렉토리에 대한 적절한 소유권과 권한 설정이 필요하며, 충분한 스토리지 공간을 확보해야 합니다.

이 문서에서는 Docker 이미지의 효과적인 관리 방법과 Docker 데이터 디렉토리 변경 방법에 대해 자세히 설명하였습니다. 이미지 검색, 다운로드, 목록 확인, 상세 정보 조회, 삭제, 태그 지정 등의 주제를 다루었으며, Docker 데이터 디렉토리의 기본 위치와 변경 방법, 주의 사항 등을 포함하고 있습니다. 이 문서를 참조하여 Docker 이미지와 데이터 디렉토리를 효율적으로 관리할 수 있을 것입니다.