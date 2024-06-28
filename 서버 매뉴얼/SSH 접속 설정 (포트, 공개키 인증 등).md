# SSH 접속 설정 (포트, 공개키 인증 등)

# 2.3. SSH 접속 설정 (포트, 공개키 인증 등)

이 문서에서는 SSH(Secure Shell) 서비스의 안전하고 효과적인 설정 방법에 대해 자세히 설명합니다. SSH는 네트워크 상에서 안전한 원격 접속과 파일 전송을 제공하는 프로토콜로, 서버 관리에 필수적인 도구입니다. SSH 설정을 최적화하고 보안 모범 사례를 적용함으로써 서버의 보안과 안정성을 크게 향상시킬 수 있습니다.

## 2.3.1. SSH 포트 변경

- 기본 SSH 포트(22)에서 다른 포트로 변경:
    
    ```bash
    sudo vi /etc/ssh/sshd_config
    
    ```
    
    - `Port` 지시자를 찾아 원하는 포트 번호로 변경
    - 예: `Port 2222`
- SSH 서비스 재시작:
    
    ```bash
    sudo systemctl restart ssh
    
    ```
    
- 방화벽 규칙 업데이트:
    
    ```bash
    sudo ufw allow 2222/tcp
    
    ```
    

기본적으로 SSH는 22번 포트를 사용하지만, 이는 공격자들이 가장 먼저 시도하는 포트이기도 합니다. SSH 포트를 변경하면 자동화된 공격을 어느 정도 방지할 수 있으며, 보안 강화에 도움이 됩니다. `/etc/ssh/sshd_config` 파일에서 `Port` 지시자를 찾아 원하는 포트 번호로 변경하고, SSH 서비스를 재시작하여 변경 사항을 적용합니다. 아울러 방화벽 규칙을 업데이트하여 새로운 포트에 대한 접속을 허용해야 합니다.

## 2.3.2. 공개키 인증 설정

- SSH 키 페어 생성:
    
    ```bash
    ssh-keygen -t ed25519 -C "your_email@example.com"
    
    ```
    
    - ED25519 알고리즘을 사용하여 강력한 SSH 키 페어 생성
    - 키 페어 저장 위치와 passphrase 입력 (선택 사항)
- 공개키를 서버에 복사:
    
    ```bash
    ssh-copy-id -i ~/.ssh/id_ed25519.pub user@server_ip
    
    ```
    
    - `ssh-copy-id` 명령을 사용하여 로컬 공개키를 서버의 `authorized_keys` 파일에 추가
- SSH 서버 설정 변경:
    
    ```bash
    sudo vi /etc/ssh/sshd_config
    
    ```
    
    - `PasswordAuthentication no` 설정을 통해 비밀번호 인증 비활성화
    - `PubkeyAuthentication yes` 설정을 통해 공개키 인증 활성화
- SSH 서비스 재시작:
    
    ```bash
    sudo systemctl restart ssh
    
    ```
    

공개키 인증은 비밀번호 인증보다 훨씬 안전한 인증 방식으로, 강력한 암호화 키를 사용하여 사용자의 신원을 확인합니다. 먼저 `ssh-keygen` 명령을 사용하여 로컬 머신에서 SSH 키 페어를 생성합니다. 생성된 공개키를 `ssh-copy-id` 명령을 통해 서버의 `authorized_keys` 파일에 추가하면, 해당 키를 가진 사용자만 SSH 접속이 가능해집니다. 이후 `/etc/ssh/sshd_config` 파일에서 비밀번호 인증을 비활성화하고 공개키 인증을 활성화하여 보안을 강화합니다.

## 2.3.3. 불필요한 SSH 프로토콜 버전 비활성화

- SSH 서버 설정 변경:
    
    ```bash
    sudo vi /etc/ssh/sshd_config
    
    ```
    
    - SSH 프로토콜 버전 2만 사용하도록 설정
    - `Protocol 2` 지시자 추가 또는 주석 해제
- SSH 서비스 재시작:
    
    ```bash
    sudo systemctl restart ssh
    
    ```
    

오래된 SSH 프로토콜 버전(SSH-1)은 알려진 취약점이 있으므로, SSH 프로토콜 버전 2만 사용하는 것이 좋습니다. `/etc/ssh/sshd_config` 파일에서 `Protocol 2`를 지정하여 SSH-2만 사용하도록 설정하고, SSH 서비스를 재시작하여 변경 사항을 적용합니다.

## 2.3.4. 비표준 시간에 로그인하는 IP 차단

- Fail2Ban 설치:
    
    ```bash
    sudo apt update
    sudo apt install fail2ban
    
    ```
    
- Fail2Ban SSH 필터 활성화:
    
    ```bash
    sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
    sudo vi /etc/fail2ban/jail.local
    
    ```
    
    - `[sshd]` 섹션을 찾아 `enabled = true`로 설정
    - `bantime`, `findtime`, `maxretry` 등의 설정 값 조정 (선택 사항)
- Fail2Ban 서비스 재시작:
    
    ```bash
    sudo systemctl restart fail2ban
    
    ```
    

Fail2Ban은 SSH를 포함한 다양한 서비스에 대한 brute-force 공격을 차단하는 데 사용되는 도구입니다. 비표준 시간(예: 심야)에 반복적인 로그인 시도가 발생하는 IP를 자동으로 차단하여 서버를 보호할 수 있습니다. Fail2Ban을 설치하고 SSH 필터를 활성화한 후, `/etc/fail2ban/jail.local` 파일에서 차단 규칙을 설정합니다. `bantime`은 차단 지속 시간, `findtime`은 탐지 기간, `maxretry`는 차단 기준 시도 횟수를 나타냅니다.

## 2.3.5. SSH 접속 제한

- 특정 사용자 또는 그룹에 대한 접속 제한:
    
    ```bash
    sudo vi /etc/ssh/sshd_config
    
    ```
    
    - `AllowUsers` 지시자를 사용하여 접속 허용 사용자 지정
    - `AllowGroups` 지시자를 사용하여 접속 허용 그룹 지정
    - 예: `AllowUsers user1 user2`, `AllowGroups sshusers`
- 루트 사용자 접속 제한:
    
    ```bash
    sudo vi /etc/ssh/sshd_config
    
    ```
    
    - `PermitRootLogin no` 설정을 통해 루트 사용자의 SSH 접속 차단
- SSH 서비스 재시작:
    
    ```bash
    sudo systemctl restart ssh
    
    ```
    

SSH 접속을 특정 사용자 또는 그룹으로 제한하면 불필요한 계정의 원격 접속을 차단할 수 있습니다. `/etc/ssh/sshd_config` 파일에서 `AllowUsers` 또는 `AllowGroups` 지시자를 사용하여 접속 허용 대상을 지정합니다. 또한 `PermitRootLogin no` 설정을 통해 루트 사용자의 직접 SSH 접속을 차단하는 것이 좋습니다. 일반 사용자로 로그인한 후 필요시 `sudo`를 사용하여 관리 작업을 수행하는 것이 보안상 안전합니다.

## 2.3.6. 로그 모니터링 및 알림 설정

- SSH 로그 파일 모니터링:
    
    ```bash
    sudo tail -f /var/log/auth.log | grep sshd
    
    ```
    
    - `tail` 명령과 `grep`을 사용하여 실시간으로 SSH 로그 모니터링
- 로그 분석 도구 사용:
    - [Logwatch](https://sourceforge.net/projects/logwatch/), [Logcheck](http://logcheck.org/) 등의 로그 분석 도구를 사용하여 SSH 로그 분석 및 보고서 생성
- 이메일 알림 설정:
    - Logwatch, Logcheck 등의 도구를 사용하여 SSH 이벤트 발생 시 관리자 이메일로 알림 전송 설정

SSH 로그를 지속적으로 모니터링하고 분석하는 것은 잠재적인 보안 위협을 탐지하고 대응하는 데 중요합니다. `tail`과 `grep` 명령을 사용하여 실시간으로 SSH 로그를 모니터링하거나, Logwatch, Logcheck와 같은 로그 분석 도구를 활용하여 자동화된 로그 분석 및 보고서 생성을 수행할 수 있습니다. 또한 이러한 도구를 통해 SSH 이벤트 발생 시 관리자에게 이메일 알림을 전송하도록 설정하면 신속한 대응이 가능해집니다.

SSH 접속 설정은 서버 보안의 핵심 요소 중 하나입니다. 적절한 설정과 보안 조치를 통해 무단 접속과 brute-force 공격을 방지하고, 사용자 인증 및 권한 관리를 강화할 수 있습니다. 이 문서에서 다룬 SSH 포트 변경, 공개키 인증, 프로토콜 버전 제한, Fail2Ban 설정, 사용자/그룹별 접속 제한, 로그 모니터링 등의 방법을 종합적으로 적용하여 SSH 접속의 보안성을 높이는 것이 좋습니다. 아울러 주기적인 점검과 로그 분석을 통해 의심스러운 활동을 탐지하고 적시에 대응함으로써 서버의 안정성과 데이터 보호를 유지할 수 있을 것입니다.