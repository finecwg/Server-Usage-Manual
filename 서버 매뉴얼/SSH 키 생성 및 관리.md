# SSH 키 생성 및 관리

# 3.2. SSH 키 생성 및 관리

SSH 키는 SSH 프로토콜에서 사용자 인증을 위해 사용되는 한 쌍의 암호화 키입니다. SSH 키를 사용하면 비밀번호 대신 개인 키와 공개 키를 사용하여 안전하게 서버에 접속할 수 있습니다. 이 문서에서는 SSH 키 생성 및 관리 방법을 자세히 다루겠습니다.

## SSH 키 쌍 생성

SSH 키 쌍을 생성하려면 SSH 클라이언트가 설치된 환경에서 다음 명령어를 사용합니다:

```
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"

```

- `t rsa`: RSA 알고리즘을 사용하여 키 쌍을 생성합니다.
- `b 4096`: 키의 비트 수를 지정합니다. 4096비트 이상을 권장합니다.
- `C "your_email@example.com"`: 키를 생성한 사용자를 식별하기 위한 주석을 추가합니다.

명령어를 실행하면 키 쌍을 저장할 위치와 키 파일명을 입력하라는 메시지가 표시됩니다. 기본값을 사용하려면 Enter 키를 누릅니다. 그러면 다음 위치에 키 쌍이 생성됩니다:

- 개인 키: `~/.ssh/id_rsa`
- 공개 키: `~/.ssh/id_rsa.pub`

키 쌍을 생성할 때 선택적으로 암호를 설정할 수 있습니다. 암호를 설정하면 키를 사용할 때마다 암호를 입력해야 합니다. 이는 추가적인 보안 계층을 제공하지만, 자동화된 스크립트에서는 불편할 수 있습니다.

## 공개 키를 서버에 등록

생성한 공개 키를 SSH 서버에 등록해야 합니다. 공개 키를 서버의 `~/.ssh/authorized_keys` 파일에 추가하면 됩니다.

1. 공개 키의 내용을 클립보드에 복사합니다:
    - macOS/Linux:
        
        ```
        pbcopy < ~/.ssh/id_rsa.pub
        
        ```
        
    - Windows (Git Bash):
        
        ```
        clip < ~/.ssh/id_rsa.pub
        
        ```
        
2. SSH를 사용하여 서버에 접속합니다:
    
    ```
    ssh username@hostname_or_ip
    
    ```
    
3. 서버에서 `~/.ssh/authorized_keys` 파일을 편집합니다:
    
    ```
    mkdir -p ~/.ssh
    echo "클립보드에 복사된 공개 키" >> ~/.ssh/authorized_keys
    
    ```
    
4. `~/.ssh` 디렉토리와 `authorized_keys` 파일의 권한을 설정합니다:
    
    ```
    chmod 700 ~/.ssh
    chmod 600 ~/.ssh/authorized_keys
    
    ```
    

이제 개인 키를 사용하여 서버에 SSH로 접속할 수 있습니다.

## SSH 에이전트

SSH 에이전트는 SSH 키를 메모리에 저장하고 관리하는 프로그램입니다. SSH 에이전트를 사용하면 키 암호를 한 번만 입력하고 여러 SSH 세션에서 키를 재사용할 수 있습니다.

### SSH 에이전트 시작

- macOS/Linux:
    
    ```
    eval "$(ssh-agent -s)"
    
    ```
    
- Windows (Git Bash):
    
    ```
    eval $(ssh-agent -s)
    
    ```
    

### SSH 에이전트에 키 추가

```
ssh-add ~/.ssh/id_rsa

```

키 암호를 입력하라는 메시지가 표시되면 암호를 입력합니다.

### SSH 에이전트에서 키 제거

```
ssh-add -d ~/.ssh/id_rsa

```

### SSH 에이전트 종료

```
ssh-agent -k

```

## 키 관리 모범 사례

- 각 서버 또는 서비스마다 별도의 SSH 키 쌍을 사용하세요. 이렇게 하면 한 키가 손상되었을 때 다른 서버에 미치는 영향을 최소화할 수 있습니다.
- 키 암호를 설정하여 키가 유출되더라도 추가적인 보안 계층을 제공하세요.
- 개인 키를 안전한 장소에 보관하고 절대로 공유하지 마세요.
- 더 이상 필요하지 않은 키는 서버의 `authorized_keys` 파일에서 제거하세요.
- 키 암호를 주기적으로 변경하세요.

## 결론

SSH 키는 서버 접속 시 강력한 인증 방법을 제공합니다. 이 문서에서는 SSH 키 쌍 생성, 공개 키 등록, SSH 에이전트 사용 및 키 관리 모범 사례에 대해 알아보았습니다. SSH 키를 적절히 관리하면 서버 접속 시 높은 수준의 보안을 유지할 수 있습니다.