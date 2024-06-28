# sudo 권한 설정

# 2.4. sudo 권한 설정

이 문서에서는 sudo(superuser do)를 사용하여 사용자 또는 그룹에 관리자 권한을 부여하는 방법에 대해 자세히 설명합니다. sudo를 통해 일반 사용자가 제한된 범위 내에서 관리 작업을 수행할 수 있도록 허용함으로써, 시스템 보안과 안정성을 유지하면서도 유연한 권한 관리가 가능해집니다.

## 2.4.1. sudo 설치 및 기본 설정

- sudo 패키지 설치:
    
    ```bash
    sudo apt update
    sudo apt install sudo
    
    ```
    
- sudo 그룹에 사용자 추가:
    
    ```bash
    sudo usermod -aG sudo username
    
    ```
    
    - 예: `sudo usermod -aG sudo jhkim`
- sudo 설정 파일 편집:
    
    ```bash
    sudo visudo
    
    ```
    
    - `/etc/sudoers` 파일을 안전하게 편집하기 위해 `visudo` 명령 사용
    - 필요한 경우 sudo 동작 관련 전역 설정 변경

sudo 패키지를 설치한 후, 관리자 권한이 필요한 사용자를 sudo 그룹에 추가합니다. 이후 `visudo` 명령을 사용하여 sudo 설정 파일(`/etc/sudoers`)을 편집할 수 있습니다. `visudo`는 파일 문법을 검사하고 안전하게 저장하는 기능을 제공하므로, 직접 `/etc/sudoers` 파일을 수정하는 것보다 안전합니다.

## 2.4.2. 사용자별 sudo 권한 설정

- `/etc/sudoers` 파일에서 사용자별 권한 설정:
    
    ```
    user1 ALL=(ALL) ALL
    user2 ALL=(ALL) NOPASSWD: /bin/systemctl restart nginx
    
    ```
    
    - `user1`은 모든 호스트에서 모든 사용자 권한으로 모든 명령 실행 가능
    - `user2`는 모든 호스트에서 모든 사용자 권한으로 비밀번호 없이 nginx 재시작 가능
- `/etc/sudoers.d/` 디렉토리에 사용자별 설정 파일 생성:
    
    ```bash
    sudo visudo -f /etc/sudoers.d/user1
    
    ```
    
    - `/etc/sudoers.d/` 디렉토리 내 개별 설정 파일을 통해 사용자별 권한 관리

사용자별로 세부적인 sudo 권한을 설정하려면 `/etc/sudoers` 파일에서 해당 사용자에 대한 설정을 추가합니다. 사용자 이름 뒤에 권한 범위를 지정하는데, `ALL`은 모든 호스트, 모든 사용자, 모든 명령을 의미합니다. `NOPASSWD` 옵션을 사용하면 sudo 실행 시 비밀번호를 요구하지 않습니다. 또한 `/etc/sudoers.d/` 디렉토리 내에 사용자별 설정 파일을 생성하여 관리할 수도 있습니다.

## 2.4.3. 그룹별 sudo 권한 설정

- `/etc/sudoers` 파일에서 그룹별 권한 설정:
    
    ```
    %group1 ALL=(ALL) ALL
    %group2 ALL=(ALL) NOPASSWD: /bin/systemctl restart apache2
    
    ```
    
    - `%group1`은 모든 호스트에서 모든 사용자 권한으로 모든 명령 실행 가능
    - `%group2`는 모든 호스트에서 모든 사용자 권한으로 비밀번호 없이 apache2 재시작 가능
- `/etc/sudoers.d/` 디렉토리에 그룹별 설정 파일 생성:
    
    ```bash
    sudo visudo -f /etc/sudoers.d/group1
    
    ```
    
    - `/etc/sudoers.d/` 디렉토리 내 개별 설정 파일을 통해 그룹별 권한 관리

그룹별로 sudo 권한을 설정하려면 `/etc/sudoers` 파일에서 그룹 이름 앞에 `%`를 붙여 설정을 추가합니다. 그룹별 권한 설정 방식은 사용자별 설정과 유사하며, 그룹에 속한 모든 사용자에게 동일한 권한이 부여됩니다. 마찬가지로 `/etc/sudoers.d/` 디렉토리 내에 그룹별 설정 파일을 생성하여 관리할 수 있습니다.

## 2.4.4. 명령별 sudo 권한 설정

- `/etc/sudoers` 파일에서 명령별 권한 설정:
    
    ```
    user1 ALL=(ALL) NOPASSWD: /bin/systemctl restart nginx, /bin/systemctl restart php-fpm
    
    ```
    
    - `user1`은 모든 호스트에서 모든 사용자 권한으로 비밀번호 없이 nginx와 php-fpm 재시작 가능
- 명령 별칭(Command Alias) 사용:
    
    ```
    Cmnd_Alias WEBMGR = /bin/systemctl restart nginx, /bin/systemctl restart php-fpm
    user1 ALL=(ALL) NOPASSWD: WEBMGR
    
    ```
    
    - `WEBMGR`이라는 별칭으로 여러 명령을 그룹화하여 권한 설정에 사용

특정 명령에 대해서만 sudo 권한을 부여하려면 `/etc/sudoers` 파일에서 사용자 또는 그룹 뒤에 해당 명령을 쉼표로 구분하여 나열합니다. 여러 명령에 대해 동일한 권한을 설정할 때는 명령 별칭(Command Alias)을 사용하면 편리합니다. 별칭을 정의한 후 권한 설정에서 별칭을 사용하면 코드 가독성과 유지보수성이 향상됩니다.

## 2.4.5. sudo 사용 로그 기록

- `/etc/sudoers` 파일에서 로그 기록 설정:
    
    ```
    Defaults logfile="/var/log/sudo.log"
    Defaults log_input, log_output
    
    ```
    
    - `logfile` 옵션을 사용하여 sudo 사용 로그 파일 경로 지정
    - `log_input`과 `log_output` 옵션을 사용하여 입력과 출력 로깅 활성화
- sudo 로그 모니터링:
    
    ```bash
    sudo tail -f /var/log/sudo.log
    
    ```
    
    - `tail` 명령을 사용하여 실시간으로 sudo 로그 모니터링

sudo 사용 내역을 추적하고 감사하기 위해서는 로그 기록 설정이 필요합니다. `/etc/sudoers` 파일에서 `Defaults` 옵션을 사용하여 로그 파일 경로와 입력/출력 로깅을 활성화할 수 있습니다. 이후 `tail` 명령을 사용하여 실시간으로 sudo 로그를 모니터링하거나, 주기적으로 로그 파일을 분석하여 비정상적인 sudo 사용 패턴을 탐지할 수 있습니다.

## 2.4.6. sudo 권한 설정 모범 사례

- 최소 권한 원칙 적용:
    - 사용자와 그룹에게 업무 수행에 필요한 최소한의 권한만 부여
    - 과도한 권한 부여는 보안 위험을 높일 수 있음
- 개별 설정 파일 활용:
    - `/etc/sudoers.d/` 디렉토리 내에 사용자 또는 그룹별 설정 파일 생성
    - 설정 파일 관리와 유지보수가 용이해짐
- 명령 별칭 사용:
    - 자주 사용되는 명령 그룹을 별칭으로 정의하여 사용
    - 설정 파일의 가독성과 유지보수성 향상
- 주기적인 권한 검토 및 조정:
    - 사용자 역할 변경, 프로젝트 종료 등에 따른 sudo 권한 조정
    - 불필요한 권한 삭제 및 최소 권한 원칙 준수

효과적인 sudo 권한 관리를 위해서는 몇 가지 모범 사례를 따르는 것이 좋습니다. 최소 권한 원칙을 적용하여 사용자와 그룹에게 필요한 최소한의 권한만 부여하고, `/etc/sudoers.d/` 디렉토리를 활용하여 개별 설정 파일을 관리하는 것이 유지보수 측면에서 유리합니다. 또한 명령 별칭을 사용하여 설정 파일의 가독성을 높이고, 주기적으로 권한을 검토 및 조정하여 불필요한 권한을 제거하는 것이 중요합니다.

sudo는 유연하고 강력한 권한 관리 도구로, 적절히 활용할 경우 시스템 보안과 안정성을 크게 향상시킬 수 있습니다. 이 문서에서 다룬 사용자 및 그룹별 권한 설정, 명령별 권한 설정, 로그 기록, 모범 사례 등을 참고하여 조직의 보안 정책과 요구 사항에 맞는 sudo 권한 관리 체계를 수립하는 것이 좋습니다. 체계적이고 일관된 sudo 권한 관리를 통해 관리 효율성을 높이고, 잠재적인 보안 위험을 최소화할 수 있을 것입니다.