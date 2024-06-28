# GPU 활용 프로그래밍 환경 설정

# 6.6. GPU 활용 프로그래밍 환경 설정

이 문서에서는 Ubuntu 서버에서 GPU를 활용한 프로그래밍 환경을 설정하는 방법에 대해 자세히 설명하겠습니다. GPU를 사용하면 높은 병렬 처리 능력을 활용하여 계산 집약적인 작업을 효율적으로 수행할 수 있습니다. 이 문서에서는 NVIDIA GPU와 CUDA 툴킷을 기준으로 설명하겠습니다.

## 사전 요구 사항

- Ubuntu 서버에 NVIDIA GPU가 설치되어 있어야 합니다.
- 서버에 적절한 전원 공급 장치와 냉각 시스템이 갖춰져 있어야 합니다.

## NVIDIA 드라이버 설치

1. 터미널을 열고 다음 명령을 실행하여 시스템 업데이트를 수행합니다:
    
    ```
    sudo apt update
    sudo apt upgrade
    
    ```
    
2. NVIDIA 드라이버 설치에 필요한 패키지를 설치합니다:
    
    ```
    sudo apt install build-essential libglvnd-dev pkg-config
    
    ```
    
3. NVIDIA 드라이버를 다운로드하고 설치합니다. 드라이버 버전은 GPU 모델에 따라 달라질 수 있습니다. 다음 명령을 실행하여 드라이버를 설치합니다:
    
    ```
    sudo apt install nvidia-driver-450
    
    ```
    
    이 예제에서는 450 버전의 드라이버를 설치합니다. 사용 중인 GPU에 맞는 드라이버 버전을 선택하세요.
    
4. 시스템을 재부팅하여 드라이버 변경 사항을 적용합니다:
    
    ```
    sudo reboot
    
    ```
    

## CUDA 툴킷 설치

1. NVIDIA 개발자 웹사이트([https://developer.nvidia.com/cuda-downloads](https://developer.nvidia.com/cuda-downloads))에서 최신 CUDA 툴킷을 다운로드합니다.
2. 다운로드한 파일의 권한을 변경하고 설치 스크립트를 실행합니다:
    
    ```
    chmod +x cuda_<version>_linux.run
    sudo ./cuda_<version>_linux.run
    
    ```
    
    `<version>`은 다운로드한 CUDA 툴킷의 버전으로 대체합니다.
    
3. 설치 과정에서 라이선스 계약에 동의하고 기본 설치 경로를 선택합니다.
4. 설치가 완료되면 CUDA 경로를 환경 변수에 추가합니다. `~/.bashrc` 파일을 편집하여 다음 내용을 추가합니다:
    
    ```
    export PATH=/usr/local/cuda/bin${PATH:+:${PATH}}
    export LD_LIBRARY_PATH=/usr/local/cuda/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
    
    ```
    
5. 변경 사항을 적용하려면 다음 명령을 실행합니다:
    
    ```
    source ~/.bashrc
    
    ```
    

## cuDNN 라이브러리 설치 (선택 사항)

cuDNN은 딥 러닝 애플리케이션의 성능을 향상시키는 NVIDIA의 GPU 가속 라이브러리입니다.

1. NVIDIA 개발자 웹사이트([https://developer.nvidia.com/cudnn](https://developer.nvidia.com/cudnn))에서 cuDNN 라이브러리를 다운로드합니다. 사용 중인 CUDA 버전에 맞는 cuDNN 버전을 선택하세요.
2. 다운로드한 cuDNN 압축 파일의 압축을 풉니다:
    
    ```
    tar -xvf cudnn-<version>-linux-x64-v<version>.tgz
    
    ```
    
    `<version>`은 다운로드한 cuDNN 버전으로 대체합니다.
    
3. cuDNN 파일을 CUDA 디렉토리로 복사합니다:
    
    ```
    sudo cp -P cuda/include/cudnn*.h /usr/local/cuda/include
    sudo cp -P cuda/lib64/libcudnn* /usr/local/cuda/lib64
    sudo chmod a+r /usr/local/cuda/include/cudnn*.h /usr/local/cuda/lib64/libcudnn*
    
    ```
    

## GPU 활용 프로그래밍 환경 테스트

1. CUDA 샘플 코드를 컴파일하고 실행하여 GPU 프로그래밍 환경을 테스트할 수 있습니다. 다음 명령을 실행하여 CUDA 샘플 코드를 빌드합니다:
    
    ```
    cd /usr/local/cuda/samples
    sudo make
    
    ```
    
2. 컴파일이 완료되면 다음 명령을 실행하여 CUDA 샘플 코드 중 하나를 실행합니다:
    
    ```
    cd /usr/local/cuda/samples/bin/x86_64/linux/release
    ./deviceQuery
    
    ```
    
    `deviceQuery` 샘플 코드는 시스템에 설치된 GPU에 대한 정보를 출력합니다.
    
3. 출력 결과에서 GPU 정보와 CUDA 버전이 올바르게 표시되는지 확인합니다.

## 추가 리소스

- NVIDIA 개발자 웹사이트([https://developer.nvidia.com/](https://developer.nvidia.com/))에서 GPU 프로그래밍에 대한 추가 정보와 문서를 찾을 수 있습니다.
- CUDA 프로그래밍 가이드([https://docs.nvidia.com/cuda/cuda-c-programming-guide/](https://docs.nvidia.com/cuda/cuda-c-programming-guide/))는 CUDA를 사용한 GPU 프로그래밍에 대한 자세한 내용을 제공합니다.
- cuDNN 개발자 가이드([https://docs.nvidia.com/deeplearning/cudnn/developer-guide/](https://docs.nvidia.com/deeplearning/cudnn/developer-guide/))는 cuDNN 라이브러리를 사용한 딥 러닝 애플리케이션 개발에 대한 정보를 제공합니다.

## 결론

이 문서에서는 Ubuntu 서버에서 GPU를 활용한 프로그래밍 환경을 설정하는 방법에 대해 설명했습니다. NVIDIA 드라이버와 CUDA 툴킷을 설치하고 환경 변수를 설정하여 GPU 프로그래밍을 위한 기본 환경을 구축할 수 있습니다. 또한 cuDNN 라이브러리를 설치하여 딥 러닝 애플리케이션의 성능을 향상시킬 수 있습니다. GPU 활용 프로그래밍 환경을 설정한 후에는 CUDA 샘플 코드를 실행하여 환경을 테스트해볼 수 있습니다. 이 문서의 단계를 따라 Ubuntu 서버에서 GPU 프로그래밍을 시작해보세요.