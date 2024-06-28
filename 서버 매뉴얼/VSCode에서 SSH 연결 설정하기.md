# VSCode에서 SSH 연결 설정하기

# 3.5. VSCode에서 SSH 연결 설정하기

Visual Studio Code(VSCode)는 강력한 코드 편집기로, SSH를 사용하여 원격 서버에 직접 연결할 수 있는 기능을 제공합니다. 이 문서에서는 VSCode에서 SSH 연결을 설정하고 원격 서버의 파일을 편집하는 방법에 대해 자세히 알아보겠습니다.

## 사전 요구 사항

- VSCode 설치: VSCode 웹사이트([https://code.visualstudio.com/](https://code.visualstudio.com/))에서 최신 버전의 VSCode를 다운로드하여 설치합니다.
- SSH 서버: 원격 서버에 SSH 서버가 설치되어 있고 실행 중이어야 합니다.
- SSH 키 (선택 사항): SSH 키 인증을 사용하려면 로컬 머신에 SSH 키 쌍을 생성하고 원격 서버에 공개 키를 등록해야 합니다.

## VSCode에서 SSH 확장 설치

1. VSCode를 실행하고 왼쪽 사이드바에서 Extensions 아이콘을 클릭합니다.
2. 검색 창에 "Remote - SSH"를 입력하고 Enter 키를 누릅니다.
3. "Remote - SSH" 확장을 찾아 "Install" 버튼을 클릭하여 설치합니다.

## SSH 연결 설정

1. VSCode의 Command Palette를 엽니다. (Windows/Linux: Ctrl+Shift+P, macOS: Cmd+Shift+P)
2. "Remote-SSH: Connect to Host..."를 입력하고 Enter 키를 누릅니다.
3. "Configure SSH Hosts..."를 선택합니다.
4. SSH 구성 파일을 선택합니다. (예: `~/.ssh/config`)
5. 새 SSH 호스트 정보를 추가합니다.
    
    ```
    Host myserver
        HostName example.com
        User john
        Port 22
        IdentityFile ~/.ssh/id_rsa
    
    ```
    
    - `Host`: SSH 연결의 별칭
    - `HostName`: 원격 서버의 호스트 이름 또는 IP 주소
    - `User`: 원격 서버의 사용자 이름
    - `Port`: SSH 포트 번호 (기본값: 22)
    - `IdentityFile`: SSH 개인 키 파일 경로 (SSH 키 인증을 사용하는 경우)
6. 파일을 저장하고 닫습니다.

## 원격 서버에 연결

1. VSCode의 Command Palette를 다시 엽니다.
2. "Remote-SSH: Connect to Host..."를 입력하고 Enter 키를 누릅니다.
3. 연결하려는 SSH 호스트를 선택합니다.
4. 비밀번호 또는 SSH 키 암호를 입력합니다. (필요한 경우)
5. 원격 서버에 성공적으로 연결되면 VSCode의 왼쪽 아래에 "SSH: myserver"와 같은 상태 표시가 나타납니다.

## 원격 파일 열기 및 편집

1. VSCode의 Explorer 창에서 "Open Folder" 버튼을 클릭합니다.
2. 원격 서버의 파일 시스템을 탐색하여 작업할 디렉토리를 선택합니다.
3. 선택한 디렉토리가 VSCode에서 열립니다.
4. 파일을 열고 편집할 수 있습니다. 변경 사항은 자동으로 원격 서버에 저장됩니다.

## 원격 터미널 사용

1. VSCode의 터미널 창을 엽니다. (View > Terminal)
2. 터미널은 원격 서버에 연결되어 있으므로 SSH 세션 내에서 명령을 실행할 수 있습니다.

## 문제 해결

- 연결 오류가 발생하는 경우 SSH 구성 파일의 설정을 확인하세요. (호스트 이름, 사용자 이름, 포트, SSH 키 경로 등)
- 방화벽 또는 보안 그룹 설정이 SSH 연결을 허용하는지 확인하세요.
- SSH 서버 로그를 확인하여 연결 문제를 파악할 수 있습니다.

## 결론

이 문서에서는 VSCode에서 SSH 연결을 설정하고 원격 서버의 파일을 편집하는 방법에 대해 알아보았습니다. VSCode의 Remote - SSH 확장을 사용하면 원격 서버에 직접 연결하여 파일을 편집하고 터미널 명령을 실행할 수 있습니다. 이 기능을 활용하면 로컬 머신에서 작업하는 것처럼 원격 서버의 프로젝트를 편리하게 개발할 수 있습니다.