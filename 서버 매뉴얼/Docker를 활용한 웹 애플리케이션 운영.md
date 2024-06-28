# Docker를 활용한 웹 애플리케이션 운영

# 4.3. Docker를 활용한 웹 애플리케이션 운영

이 문서에서는 Ubuntu 서버에서 Docker를 활용하여 다양한 웹 애플리케이션을 운영하는 방법에 대해 자세히 설명합니다. Docker는 애플리케이션을 컨테이너화하여 배포 및 관리를 단순화하는 강력한 도구로, 웹 애플리케이션 운영에 많은 이점을 제공합니다. 이 문서에서는 WordPress, phpMyAdmin, Uptime Kuma, Filebrowser, Dockge, Obsidian Remote 등의 실제 운영 사례를 통해 Docker 기반 웹 애플리케이션 운영 방법을 살펴보겠습니다.

## 4.3.1. WordPress

WordPress는 가장 널리 사용되는 콘텐츠 관리 시스템(CMS) 중 하나로, Docker를 사용하여 쉽게 배포할 수 있습니다.

### 4.3.1.1. Docker Compose를 사용한 WordPress 배포

- `docker-compose.yml` 파일 작성:
    
    ```yaml
    version: '3'
    services:
      wordpress:
        image: wordpress
        restart: always
        ports:
          - 8080:80
        environment:
          WORDPRESS_DB_HOST: db
          WORDPRESS_DB_USER: wordpress
          WORDPRESS_DB_PASSWORD: wordpress_password
          WORDPRESS_DB_NAME: wordpress
        volumes:
          - wordpress_data:/var/www/html
      db:
        image: mysql:5.7
        restart: always
        environment:
          MYSQL_ROOT_PASSWORD: root_password
          MYSQL_DATABASE: wordpress
          MYSQL_USER: wordpress
          MYSQL_PASSWORD: wordpress_password
        volumes:
          - db_data:/var/lib/mysql
    volumes:
      wordpress_data:
      db_data:
    
    ```
    
- WordPress 컨테이너 시작:
    
    ```bash
    docker-compose up -d
    
    ```
    

Docker Compose를 사용하면 WordPress와 MySQL 데이터베이스를 함께 배포할 수 있습니다. `docker-compose.yml` 파일에 WordPress와 MySQL 서비스를 정의하고, 필요한 환경 변수와 볼륨을 설정합니다. `docker-compose up -d` 명령어를 사용하여 컨테이너를 시작하면 WordPress가 자동으로 설정되고 실행됩니다.

### 4.3.1.2. WordPress 사용

- 웹 브라우저에서 `http://<server-ip>:8080` 접속
- WordPress 설치 마법사 진행
- 관리자 계정 생성 및 로그인
- 콘텐츠 작성 및 관리

WordPress 컨테이너가 실행되면 웹 브라우저를 통해 접속할 수 있습니다. WordPress 설치 마법사를 진행하고 관리자 계정을 생성한 후, 콘텐츠를 작성하고 관리할 수 있습니다.

## 4.3.2. phpMyAdmin

phpMyAdmin은 웹 기반 MySQL 및 MariaDB 관리 도구로, Docker를 사용하여 쉽게 배포할 수 있습니다.

### 4.3.2.1. Docker를 사용한 phpMyAdmin 배포

- phpMyAdmin 컨테이너 실행:
    
    ```bash
    docker run --name phpmyadmin -d --link mysql:db -p 8081:80 phpmyadmin/phpmyadmin
    
    ```
    

Docker의 `run` 명령어를 사용하여 phpMyAdmin 컨테이너를 실행할 수 있습니다. `--link` 옵션을 사용하여 MySQL 컨테이너와 연결하고, `-p` 옵션을 사용하여 호스트의 포트를 컨테이너의 포트에 매핑합니다.

### 4.3.2.2. phpMyAdmin 사용

- 웹 브라우저에서 `http://<server-ip>:8081` 접속
- MySQL 서버 호스트명, 사용자 이름, 비밀번호 입력
- 데이터베이스 관리 작업 수행

phpMyAdmin 컨테이너가 실행되면 웹 브라우저를 통해 접속할 수 있습니다. MySQL 서버의 호스트명, 사용자 이름, 비밀번호를 입력하여 로그인한 후, 데이터베이스 관리 작업을 수행할 수 있습니다.

## 4.3.3. Uptime Kuma

Uptime Kuma는 웹 사이트 및 서비스의 가동 시간을 모니터링하는 오픈소스 도구로, Docker를 사용하여 쉽게 배포할 수 있습니다.

### 4.3.3.1. Docker를 사용한 Uptime Kuma 배포

- Uptime Kuma 컨테이너 실행:
    
    ```bash
    docker run -d --name uptime-kuma -p 3001:3001 -v uptime-kuma:/app/data louislam/uptime-kuma:1
    
    ```
    

Docker의 `run` 명령어를 사용하여 Uptime Kuma 컨테이너를 실행할 수 있습니다. `-p` 옵션을 사용하여 호스트의 포트를 컨테이너의 포트에 매핑하고, `-v` 옵션을 사용하여 데이터 지속성을 위한 볼륨을 마운트합니다.

### 4.3.3.2. Uptime Kuma 사용

- 웹 브라우저에서 `http://<server-ip>:3001` 접속
- 관리자 계정 생성
- 모니터링할 웹 사이트 및 서비스 추가
- 가동 시간 및 응답 시간 확인

Uptime Kuma 컨테이너가 실행되면 웹 브라우저를 통해 접속할 수 있습니다. 관리자 계정을 생성하고, 모니터링할 웹 사이트와 서비스를 추가한 후, 가동 시간과 응답 시간을 확인할 수 있습니다.

## 4.3.4. Filebrowser

Filebrowser는 웹 기반 파일 관리자로, Docker를 사용하여 쉽게 배포할 수 있습니다.

### 4.3.4.1. Docker를 사용한 Filebrowser 배포

- Filebrowser 컨테이너 실행:
    
    ```bash
    docker run -d --name filebrowser -v /path/to/data:/srv -p 8088:80 hurlenko/filebrowser
    
    ```
    

Docker의 `run` 명령어를 사용하여 Filebrowser 컨테이너를 실행할 수 있습니다. `-v` 옵션을 사용하여 호스트의 디렉토리를 컨테이너의 `/srv` 디렉토리에 마운트하고, `-p` 옵션을 사용하여 호스트의 포트를 컨테이너의 포트에 매핑합니다.

### 4.3.4.2. Filebrowser 사용

- 웹 브라우저에서 `http://<server-ip>:8088` 접속
- 초기 설정 및 관리자 계정 생성
- 파일 및 디렉토리 관리

Filebrowser 컨테이너가 실행되면 웹 브라우저를 통해 접속할 수 있습니다. 초기 설정을 진행하고 관리자 계정을 생성한 후, 파일과 디렉토리를 관리할 수 있습니다.

## 4.3.5. Dockge

Dockge는 Docker 컨테이너 관리를 위한 웹 기반 사용자 인터페이스로, Docker를 사용하여 쉽게 배포할 수 있습니다.

### 4.3.5.1. Docker를 사용한 Dockge 배포

- Dockge 컨테이너 실행:
    
    ```bash
    docker run -d --name dockge -v /var/run/docker.sock:/var/run/docker.sock -p 5001:5001 louislam/dockge:1
    
    ```
    

Docker의 `run` 명령어를 사용하여 Dockge 컨테이너를 실행할 수 있습니다. `-v` 옵션을 사용하여 Docker 소켓을 컨테이너에 마운트하고, `-p` 옵션을 사용하여 호스트의 포트를 컨테이너의 포트에 매핑합니다.

### 4.3.5.2. Dockge 사용

- 웹 브라우저에서 `http://<server-ip>:5001` 접속
- Docker 컨테이너 목록 확인
- 컨테이너 시작, 중지, 재시작, 삭제 등 관리 작업 수행

Dockge 컨테이너가 실행되면 웹 브라우저를 통해 접속할 수 있습니다. Docker 컨테이너 목록을 확인하고, 컨테이너 시작, 중지, 재시작, 삭제 등의 관리 작업을 수행할 수 있습니다.

## 4.3.6. Obsidian Remote

Obsidian Remote는 Obsidian 노트 앱을 위한 원격 액세스 솔루션으로, Docker를 사용하여 쉽게 배포할 수 있습니다.

### 4.3.6.1. Docker를 사용한 Obsidian Remote 배포

- Obsidian Remote 컨테이너 실행:
    
    ```bash
    docker run -d --name obsidian-remote -e PASSWORD=your_password -v /path/to/vault:/vault -p 8080:8080 ghcr.io/sytone/obsidian-remote:latest
    
    ```
    

Docker의 `run` 명령어를 사용하여 Obsidian Remote 컨테이너를 실행할 수 있습니다. `-e` 옵션을 사용하여 액세스 비밀번호를 설정하고, `-v` 옵션을 사용하여 Obsidian 볼트 디렉토리를 컨테이너에 마운트합니다. `-p` 옵션을 사용하여 호스트의 포트를 컨테이너의 포트에 매핑합니다.

### 4.3.6.2. Obsidian Remote 사용

- 웹 브라우저에서 `http://<server-ip>:8080` 접속
- 비밀번호 입력 및 로그인
- Obsidian 노트 앱을 통해 원격 볼트 액세스

Obsidian Remote 컨테이너가 실행되면 웹 브라우저를 통해 접속할 수 있습니다. 설정한 비밀번호를 입력하여 로그인한 후, Obsidian 노트 앱을 사용하여 원격 볼트에 액세스할 수 있습니다.

## 4.3.7. 기타 웹 애플리케이션

Docker를 활용하여 다양한 웹 애플리케이션을 운영할 수 있습니다. 위에서 소개한 애플리케이션 외에도 다음과 같은 예시가 있습니다:

- Nextcloud: 개인 클라우드 스토리지 및 협업 플랫폼
- Gitea: 자체 호스팅 Git 서비스
- Grafana: 데이터 시각화 및 모니터링 플랫폼
- Mattermost: 오픈소스 팀 커뮤니케이션 플랫폼
- Portainer: Docker 컨테이너 관리를 위한 웹 기반 사용자 인터페이스

각 애플리케이션의 공식 Docker 이미지와 문서를 참조하여 Docker를 사용한 배포 및 운영 방법을 확인할 수 있습니다.

이 문서에서는 Docker를 활용하여 다양한 웹 애플리케이션을 운영하는 방법에 대해 자세히 다루었습니다. WordPress, phpMyAdmin, Uptime Kuma, Filebrowser, Dockge, Obsidian Remote 등의 실제 운영 사례를 통해 Docker 기반 웹 애플리케이션 배포 및 사용 방법을 살펴보았습니다. Docker의 강력한 기능을 활용하여 웹 애플리케이션 운영을 단순화하고 효율성을 높일 수 있습니다.