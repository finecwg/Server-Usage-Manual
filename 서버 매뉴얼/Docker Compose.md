# Docker Compose

docker-compose.yml 파일 작성

멀티 컨테이너 애플리케이션 정의

서비스 시작/중지 및 관리

Dockge를 활용한 docker compose 관리

# 10.2.5. Docker Compose

Docker Compose는 여러 개의 Docker 컨테이너를 정의하고 실행하기 위한 도구입니다. YAML 파일을 사용하여 애플리케이션의 서비스를 구성하고, 단일 명령으로 모든 서비스를 시작하고 관리할 수 있습니다. 이 문서에서는 Docker Compose의 기본 개념과 사용 방법에 대해 자세히 설명하겠습니다.

## Docker Compose 설치

Docker Compose를 설치하려면 다음 단계를 수행하세요:

1. 터미널을 열고 다음 명령을 실행하여 Docker Compose의 최신 버전을 다운로드합니다:
    
    ```
    sudo curl -L "<https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$>(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
    
    ```
    
2. 다운로드한 바이너리 파일에 실행 권한을 부여합니다:
    
    ```
    sudo chmod +x /usr/local/bin/docker-compose
    
    ```
    
3. Docker Compose 버전을 확인하여 설치가 올바르게 되었는지 확인합니다:
    
    ```
    docker-compose --version
    
    ```
    

## docker-compose.yml 파일 작성

Docker Compose는 `docker-compose.yml` 파일을 사용하여 애플리케이션의 서비스를 정의합니다. 이 파일에는 서비스의 이미지, 포트 매핑, 볼륨, 환경 변수 등을 지정할 수 있습니다.

다음은 간단한 `docker-compose.yml` 파일의 예시입니다:

```yaml
version: '3'
services:
  web:
    image: nginx
    ports:
      - "80:80"
    volumes:
      - ./html:/usr/share/nginx/html
  db:
    image: mysql
    environment:
      MYSQL_ROOT_PASSWORD: example
    volumes:
      - ./mysql-data:/var/lib/mysql

```

위 예시에서는 `web`과 `db` 두 개의 서비스를 정의하고 있습니다. `web` 서비스는 Nginx 이미지를 사용하고, 호스트의 80번 포트를 컨테이너의 80번 포트로 매핑합니다. 또한 호스트의 `./html` 디렉토리를 컨테이너의 `/usr/share/nginx/html` 디렉토리로 마운트합니다.

`db` 서비스는 MySQL 이미지를 사용하고, 환경 변수로 루트 비밀번호를 설정합니다. 호스트의 `./mysql-data` 디렉토리를 컨테이너의 `/var/lib/mysql` 디렉토리로 마운트하여 데이터 지속성을 유지합니다.

## Docker Compose 명령어

Docker Compose는 다양한 명령어를 제공하여 서비스를 관리할 수 있습니다.

- `docker-compose up`: `docker-compose.yml` 파일에 정의된 모든 서비스를 시작합니다. `d` 옵션을 사용하여 백그라운드에서 실행할 수 있습니다.
- `docker-compose down`: 모든 서비스를 중지하고 관련 컨테이너, 네트워크, 볼륨을 제거합니다.
- `docker-compose ps`: 현재 실행 중인 서비스의 상태를 확인합니다.
- `docker-compose logs`: 서비스의 로그를 확인합니다. 특정 서비스의 로그를 보려면 서비스 이름을 인자로 전달합니다.
- `docker-compose exec`: 실행 중인 컨테이너에서 명령을 실행합니다. 예를 들어, `docker-compose exec web bash`는 `web` 서비스의 컨테이너에서 Bash 셸을 실행합니다.

## 서비스 간 통신

Docker Compose는 자동으로 동일한 `docker-compose.yml` 파일에 정의된 서비스 간에 네트워크를 생성하여 서비스 간 통신을 가능하게 합니다. 각 서비스는 서비스 이름을 호스트 이름으로 사용하여 다른 서비스에 접근할 수 있습니다.

예를 들어, `web` 서비스에서 `db` 서비스의 MySQL 데이터베이스에 접근하려면 다음과 같이 호스트 이름으로 `db`를 사용할 수 있습니다:

```
mysql -h db -u root -p

```

## 환경 변수 사용

Docker Compose에서는 환경 변수를 사용하여 서비스 구성을 동적으로 조정할 수 있습니다. `docker-compose.yml` 파일에서 환경 변수를 정의하고, 서비스 내에서 해당 변수를 사용할 수 있습니다.

```yaml
version: '3'
services:
  web:
    image: nginx
    environment:
      - NGINX_PORT=80
    ports:
      - "${NGINX_PORT}:80"

```

위 예시에서는 `NGINX_PORT` 환경 변수를 정의하고, `ports` 설정에서 해당 변수를 사용하여 포트 매핑을 동적으로 구성합니다.

환경 변수는 `docker-compose.yml` 파일 내에서 정의하거나, 별도의 `.env` 파일에서 정의할 수 있습니다.

## 볼륨 사용

Docker Compose에서는 볼륨을 사용하여 데이터 지속성을 유지할 수 있습니다. 호스트 머신의 디렉토리를 컨테이너의 특정 디렉토리로 마운트하거나, 명명된 볼륨을 사용할 수 있습니다.

```yaml
version: '3'
services:
  web:
    image: nginx
    volumes:
      - ./html:/usr/share/nginx/html
      - data-volume:/var/lib/nginx
volumes:
  data-volume:

```

위 예시에서는 호스트의 `./html` 디렉토리를 컨테이너의 `/usr/share/nginx/html` 디렉토리로 마운트하고, 명명된 볼륨 `data-volume`을 컨테이너의 `/var/lib/nginx` 디렉토리로 마운트합니다.

## 결론

Docker Compose는 여러 개의 Docker 컨테이너로 구성된 애플리케이션을 쉽게 정의하고 관리할 수 있는 강력한 도구입니다. `docker-compose.yml` 파일을 사용하여 서비스를 정의하고, 단일 명령으로 모든 서비스를 시작하고 관리할 수 있습니다. 이 문서에서는 Docker Compose의 기본 개념과 사용 방법, 서비스 간 통신, 환경 변수 사용, 볼륨 사용 등에 대해 설명했습니다. Docker Compose를 활용하여 복잡한 애플리케이션을 효과적으로 관리해보세요.