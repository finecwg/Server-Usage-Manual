# Xorg 및 LightDM 설정 및 사용

# 4.1. Xorg 및 LightDM 설정 및 사용

이 문서에서는 Ubuntu 서버에서 Xorg 디스플레이 서버와 LightDM 디스플레이 매니저를 설정하고 사용하는 방법에 대해 자세히 설명하겠습니다. Xorg와 LightDM을 사용하면 Ubuntu 서버에 그래픽 사용자 인터페이스(GUI)를 추가할 수 있습니다.

## Xorg 설치

1. 터미널을 열고 다음 명령을 실행하여 Xorg를 설치합니다:
    
    ```
    sudo apt update
    sudo apt install xorg
    
    ```
    
2. 설치가 완료되면 `startx` 명령을 사용하여 Xorg를 시작할 수 있습니다.

## LightDM 설치

1. 터미널에서 다음 명령을 실행하여 LightDM을 설치합니다:
    
    ```
    sudo apt install lightdm
    
    ```
    
2. 설치 중에 디스플레이 매니저를 선택하라는 메시지가 표시되면 LightDM을 선택합니다.

## Xorg 설정

기본적으로 Xorg는 자동 탐지 모드로 실행되어 연결된 디스플레이와 입력 장치를 감지합니다. 그러나 경우에 따라 사용자 정의 설정이 필요할 수 있습니다.

1. Xorg 설정 파일을 만들려면 다음 명령을 실행합니다:
    
    ```
    sudo Xorg :0 -configure
    
    ```
    
    이 명령은 `/root/xorg.conf.new` 파일을 생성합니다.
    
2. 생성된 설정 파일을 `/etc/X11/xorg.conf`로 복사합니다:
    
    ```
    sudo cp /root/xorg.conf.new /etc/X11/xorg.conf
    
    ```
    
3. `/etc/X11/xorg.conf` 파일을 편집하여 필요한 설정을 수정합니다. 예를 들어, 디스플레이 해상도, 입력 장치 설정 등을 변경할 수 있습니다.

## LightDM 설정

LightDM의 설정은 `/etc/lightdm/lightdm.conf` 파일에서 수정할 수 있습니다.

1. 터미널에서 다음 명령을 사용하여 설정 파일을 엽니다:
    
    ```
    sudo nano /etc/lightdm/lightdm.conf
    
    ```
    
2. 파일에서 필요한 설정을 수정합니다. 예를 들어:
    - `autologin-user`: 자동 로그인할 사용자 계정을 설정합니다.
    - `greeter-session`: 사용할 그리터(로그인 화면)를 설정합니다.
    - `user-session`: 기본 사용자 세션을 설정합니다.
3. 변경 사항을 저장하고 파일을 닫습니다.

## 데스크톱 환경 설치

Ubuntu 서버에 데스크톱 환경을 설치하면 그래픽 사용자 인터페이스를 사용할 수 있습니다.

1. 터미널에서 다음 명령 중 하나를 실행하여 원하는 데스크톱 환경을 설치합니다:
    - GNOME:
        
        ```
        sudo apt install ubuntu-gnome-desktop
        
        ```
        
    - KDE:
        
        ```
        sudo apt install kubuntu-desktop
        
        ```
        
    - Xfce:
        
        ```
        sudo apt install xubuntu-desktop
        
        ```
        
2. 설치가 완료되면 시스템을 재부팅합니다.

## LightDM 사용

1. 시스템이 부팅되면 LightDM 로그인 화면이 나타납니다.
2. 사용자 이름과 비밀번호를 입력하여 로그인합니다.
3. 로그인하면 선택한 데스크톱 환경이 시작됩니다.

## 문제 해결

- Xorg가 제대로 시작되지 않는 경우 `/var/log/Xorg.0.log` 파일을 확인하여 오류 메시지를 확인할 수 있습니다.
- LightDM이 시작되지 않는 경우 `/var/log/lightdm/lightdm.log` 파일을 확인하세요.
- 그래픽 드라이버 문제가 발생하는 경우 해당 그래픽 카드에 맞는 드라이버를 설치해야 할 수 있습니다.

## 결론

이 문서에서는 Ubuntu 서버에서 Xorg 디스플레이 서버와 LightDM 디스플레이 매니저를 설정하고 사용하는 방법에 대해 알아보았습니다. Xorg와 LightDM을 사용하면 Ubuntu 서버에 그래픽 사용자 인터페이스를 추가할 수 있으며, 다양한 데스크톱 환경을 선택하여 설치할 수 있습니다. 이 문서의 단계를 따라 Ubuntu 서버에서 GUI를 설정하고 사용해보세요.