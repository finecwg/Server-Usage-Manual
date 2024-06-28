# Rstudio Server

# 4.2. RStudio Server

이 문서에서는 Ubuntu 서버에 RStudio Server를 설치하고 설정하는 방법에 대해 자세히 설명합니다. RStudio Server는 R 프로그래밍 언어를 위한 웹 기반 통합 개발 환경(IDE)으로, 서버에 설치하여 여러 사용자가 원격으로 액세스할 수 있습니다. 이를 통해 데이터 과학자, 통계학자, 연구원 등이 협업하고 효율적으로 작업할 수 있습니다.

## 4.2.1. RStudio Server 설치

### 4.2.1.1. R 설치

- R 저장소 추가:
    
    ```bash
    sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9
    sudo add-apt-repository 'deb <https://cloud.r-project.org/bin/linux/ubuntu> focal-cran40/'
    
    ```
    
- R 설치:
    
    ```bash
    sudo apt update
    sudo apt install r-base
    
    ```
    

RStudio Server를 설치하기 전에 먼저 R을 설치해야 합니다. R 프로젝트의 공식 저장소를 추가하고 `apt` 명령어를 사용하여 R을 설치합니다.

### 4.2.1.2. RStudio Server 설치

- RStudio Server 다운로드:
    
    ```bash
    wget <https://download2.rstudio.org/server/bionic/amd64/rstudio-server-2023.12.1+402-amd64.deb>
    
    ```
    
- RStudio Server 설치:
    
    ```bash
    sudo apt install ./rstudio-server-2023.12.1+402-amd64.deb
    
    ```
    

RStudio Server의 최신 버전을 다운로드하고 `apt` 명령어를 사용하여 설치 파일을 설치합니다. 설치 과정에서 필요한 종속성 패키지가 자동으로 함께 설치됩니다.

## 4.2.2. RStudio Server 설정

### 4.2.2.1. 포트 설정

- 기본 포트: 8787
- 포트 변경 방법:
    - `/etc/rstudio/rserver.conf` 파일 편집
    - `www-port=8787` 줄을 찾아 원하는 포트 번호로 변경
    - RStudio Server 재시작: `sudo systemctl restart rstudio-server`

RStudio Server는 기본적으로 8787 포트를 사용하여 웹 인터페이스를 제공합니다. 필요에 따라 `/etc/rstudio/rserver.conf` 파일을 편집하여 포트를 변경할 수 있습니다.

### 4.2.2.2. 사용자 액세스 제어

- 사용자 계정 생성:
    
    ```bash
    sudo adduser rstudio-user
    
    ```
    
- 사용자 비밀번호 설정:
    
    ```bash
    sudo passwd rstudio-user
    
    ```
    
- RStudio Server 웹 인터페이스 접속:
    - 웹 브라우저에서 `http://<server-ip>:8787` 접속
    - 생성한 사용자 계정으로 로그인

RStudio Server는 시스템 사용자 계정을 사용하여 액세스를 제어합니다. 새로운 사용자 계정을 생성하고 비밀번호를 설정하여 RStudio Server에 액세스할 수 있도록 합니다. 웹 브라우저를 통해 RStudio Server의 웹 인터페이스에 접속하고 생성한 사용자 계정으로 로그인합니다.

### 4.2.2.3. 패키지 설치 및 관리

- R 패키지 설치:
    - RStudio Server 웹 인터페이스에서 `install.packages("package-name")` 명령 사용
    - 또는 터미널에서 `sudo su - -c "R -e \\"install.packages('package-name', repos='<https://cran.rstudio.com/>')\\""` 명령 사용
- 시스템 전역 패키지 설치:
    - 터미널에서 `sudo su - -c "R -e \\"install.packages('package-name', repos='<https://cran.rstudio.com/>')\\""`

RStudio Server에서 R 패키지를 설치하고 관리할 수 있습니다. 웹 인터페이스에서 직접 `install.packages()` 함수를 사용하거나, 터미널에서 `sudo` 명령어와 함께 R 명령을 실행하여 패키지를 설치할 수 있습니다. 시스템 전역 패키지를 설치할 때는 `sudo` 명령어를 사용합니다.

### 4.2.2.4. 추가 설정 옵션

- `/etc/rstudio/rserver.conf` 파일을 편집하여 다양한 설정 옵션 조정 가능
    - `auth-timeout`: 인증 세션 유지 시간 (분)
    - `auth-stay-signed-in-days`: 로그인 상태 유지 기간 (일)
    - `rsession-which-r`: 사용할 R 실행 파일 경로
    - `rsession-ld-library-path`: R 패키지 라이브러리 경로
- 설정 파일 변경 후 RStudio Server 재시작:
    
    ```bash
    sudo systemctl restart rstudio-server
    
    ```
    

`/etc/rstudio/rserver.conf` 파일에는 RStudio Server의 다양한 설정 옵션이 포함되어 있습니다. 이 파일을 편집하여 인증 세션 유지 시간, 로그인 상태 유지 기간, 사용할 R 버전, 패키지 라이브러리 경로 등을 조정할 수 있습니다. 설정 파일을 변경한 후에는 RStudio Server를 재시작하여 변경 사항을 적용해야 합니다.

## 4.2.3. RStudio Server 사용

### 4.2.3.1. 웹 인터페이스 액세스

- 웹 브라우저에서 `http://<server-ip>:8787` 접속
- 사용자 계정으로 로그인
- RStudio IDE 인터페이스 사용 가능

사용자는 웹 브라우저를 통해 RStudio Server의 웹 인터페이스에 액세스할 수 있습니다. 서버의 IP 주소와 설정된 포트 번호를 사용하여 접속하고, 생성한 사용자 계정으로 로그인하면 RStudio IDE 인터페이스를 사용할 수 있습니다.

### 4.2.3.2. 프로젝트 관리

- 새 프로젝트 생성:
    - `File` > `New Project...` 메뉴 선택
    - 프로젝트 유형 및 설정 선택
- 기존 프로젝트 열기:
    - `File` > `Open Project...` 메뉴 선택
    - 프로젝트 디렉토리 선택
- 프로젝트 간 전환:
    - `File` > `Open Project...` 메뉴 선택
    - 전환할 프로젝트 선택

RStudio Server에서는 프로젝트 단위로 작업을 관리할 수 있습니다. 새 프로젝트를 생성하거나 기존 프로젝트를 열어 작업할 수 있으며, 프로젝트 간 전환도 쉽게 할 수 있습니다.

### 4.2.3.3. 협업 기능

- 프로젝트 공유:
    - 프로젝트 디렉토리를 다른 사용자와 공유
    - 버전 관리 시스템(예: Git)을 사용하여 협업
- 코드 공유:
    - RStudio Server의 `Share` 기능을 사용하여 코드 공유
    - 공유된 코드에 댓글 및 제안 사항 추가 가능

RStudio Server는 협업 기능을 제공하여 여러 사용자가 함께 작업할 수 있습니다. 프로젝트 디렉토리를 공유하거나 버전 관리 시스템을 사용하여 협업할 수 있으며, RStudio Server의 `Share` 기능을 통해 코드를 공유하고 피드백을 주고받을 수 있습니다.

## 4.2.4. RStudio Server 관리

### 4.2.4.1. 서비스 관리

- RStudio Server 시작:
    
    ```bash
    sudo systemctl start rstudio-server
    
    ```
    
- RStudio Server 중지:
    
    ```bash
    sudo systemctl stop rstudio-server
    
    ```
    
- RStudio Server 재시작:
    
    ```bash
    sudo systemctl restart rstudio-server
    
    ```
    
- RStudio Server 상태 확인:
    
    ```bash
    sudo systemctl status rstudio-server
    
    ```
    

RStudio Server는 시스템 서비스로 실행되므로 `systemctl` 명령어를 사용하여 관리할 수 있습니다. 서비스를 시작, 중지, 재시작할 수 있으며, 현재 상태를 확인할 수도 있습니다.

### 4.2.4.2. 로그 파일

- RStudio Server 로그 파일 위치:
    - `/var/log/rstudio-server/rserver.log`
- 로그 파일 모니터링:
    
    ```bash
    sudo tail -f /var/log/rstudio-server/rserver.log
    
    ```
    

RStudio Server의 로그 파일은 `/var/log/rstudio-server/rserver.log`에 위치합니다. `tail` 명령어를 사용하여 로그 파일을 실시간으로 모니터링할 수 있습니다.

### 4.2.4.3. 백업 및 복원

- RStudio Server 설정 파일 백업:
    
    ```bash
    sudo cp /etc/rstudio/rserver.conf /path/to/backup/
    
    ```
    
- 사용자 홈 디렉토리 백업:
    
    ```bash
    sudo tar -czf /path/to/backup/rstudio-user-homes.tar.gz /home/rstudio-user/
    
    ```
    
- 백업 파일로부터 복원:
    - 설정 파일 복원: `sudo cp /path/to/backup/rserver.conf /etc/rstudio/`
    - 사용자 홈 디렉토리 복원: `sudo tar -xzf /path/to/backup/rstudio-user-homes.tar.gz -C /`

RStudio Server의 설정 파일과 사용자 데이터를 정기적으로 백업하는 것이 중요합니다. 설정 파일은 `cp` 명령어를 사용하여 백업할 수 있으며, 사용자 홈 디렉토리는 `tar` 명령어를 사용하여 압축 및 백업할 수 있습니다. 백업 파일로부터 설정과 데이터를 복원할 때는 해당 명령어를 사용합니다.

이 문서에서는 Ubuntu 서버에 RStudio Server를 설치하고 설정하는 방법, 사용자 관리, 프로젝트 관리, 협업 기능, 서비스 관리, 로그 모니터링, 백업 및 복원에 대해 자세히 다루었습니다. RStudio Server를 효과적으로 구축하고 관리함으로써 데이터 분석 및 R 프로그래밍 작업을 위한 안정적이고 효율적인 환경을 제공할 수 있습니다.