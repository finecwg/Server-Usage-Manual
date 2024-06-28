# GPU 활용을 위한 NVIDIA Docker 설정

# 10.2.6. GPU 활용을 위한 NVIDIA Docker 설정

NVIDIA Docker는 Docker 컨테이너 내에서 NVIDIA GPU를 활용할 수 있도록 도와주는 도구입니다. 딥 러닝, 머신 러닝, 고성능 컴퓨팅 등의 분야에서 GPU 가속이 필요한 경우 NVIDIA Docker를 사용하여 컨테이너화된 환경에서 GPU 자원을 효과적으로 사용할 수 있습니다. 이 문서에서는 NVIDIA Docker를 설정하고 사용하는 방법에 대해 자세히 설명하겠습니다.

## 사전 요구 사항

NVIDIA Docker를 사용하려면 다음과 같은 사전 요구 사항을 충족해야 합니다:

- NVIDIA GPU가 장착된 호스트 시스템
- NVIDIA 드라이버가 설치되어 있어야 함 (최소 버전: 418.81.07)
- Docker가 설치되어 있어야 함 (최소 버전: 19.03)

## NVIDIA Docker 설치

1. 패키지 저장소 설정:
    - `distribution=$(. /etc/os-release;echo $ID$VERSION_ID)` 명령을 실행하여 운영 체제 배포판 정보를 확인합니다.
    - NVIDIA Docker 저장소를 추가합니다:
        
        ```
        curl -s -L <https://nvidia.github.io/nvidia-docker/gpgkey> | sudo apt-key add -
        curl -s -L <https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list> | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
        
        ```
        
2. NVIDIA Docker 패키지 설치:
    
    ```
    sudo apt-get update
    sudo apt-get install -y nvidia-docker2
    
    ```
    
3. Docker 데몬 재시작:
    
    ```
    sudo systemctl restart docker
    
    ```
    
4. 설치 확인:
    
    ```
    sudo docker run --rm --gpus all nvidia/cuda:11.0-base nvidia-smi
    
    ```
    
    위 명령은 NVIDIA Docker를 사용하여 CUDA 11.0 기반 컨테이너를 실행하고, `nvidia-smi` 명령을 통해 GPU 정보를 출력합니다. 출력 결과에 GPU 정보가 표시되면 설치가 올바르게 된 것입니다.
    

## NVIDIA Docker 사용

NVIDIA Docker를 사용하여 컨테이너를 실행할 때는 `docker run` 명령에 `--gpus` 옵션을 추가하여 GPU 자원을 할당합니다.

```
docker run --gpus <gpu-options> <image> <command>

```

- `<gpu-options>`: GPU 할당 옵션을 지정합니다. `all`을 사용하면 호스트의 모든 GPU를 컨테이너에 할당하고, `device=<gpu-id>`를 사용하면 특정 GPU만 할당할 수 있습니다.
- `<image>`: 사용할 Docker 이미지를 지정합니다. NVIDIA에서 제공하는 공식 CUDA 이미지를 사용하거나, 사용자 정의 이미지를 사용할 수 있습니다.
- `<command>`: 컨테이너 내에서 실행할 명령을 지정합니다.

예를 들어, 다음 명령은 CUDA 11.0 기반 컨테이너를 실행하고, 호스트의 모든 GPU를 할당하며, `nvidia-smi` 명령을 실행합니다:

```
docker run --gpus all nvidia/cuda:11.0-base nvidia-smi

```

## GPU 가속 애플리케이션 실행

NVIDIA Docker를 사용하여 GPU 가속이 필요한 애플리케이션을 컨테이너 내에서 실행할 수 있습니다. 다음은 TensorFlow와 PyTorch를 사용하는 예시입니다:

1. TensorFlow:
    
    ```
    docker run --gpus all -it --rm tensorflow/tensorflow:latest-gpu python -c "import tensorflow as tf; print(tf.reduce_sum(tf.random.normal([1000, 1000])))"
    
    ```
    
    위 명령은 TensorFlow의 최신 GPU 버전 이미지를 사용하여 컨테이너를 실행하고, 간단한 TensorFlow 코드를 실행합니다.
    
2. PyTorch:
    
    ```
    docker run --gpus all -it --rm pytorch/pytorch:latest python -c "import torch; print(torch.cuda.is_available())"
    
    ```
    
    위 명령은 PyTorch의 최신 이미지를 사용하여 컨테이너를 실행하고, GPU 사용 가능 여부를 출력합니다.
    

## Dockerfile에서 NVIDIA Docker 사용

Dockerfile을 작성할 때 NVIDIA Docker를 사용하려면 기본 이미지로 NVIDIA CUDA 이미지를 사용하고, `CUDA_VISIBLE_DEVICES` 환경 변수를 설정하여 GPU 할당을 제어할 수 있습니다.

```
FROM nvidia/cuda:11.0-base

ENV CUDA_VISIBLE_DEVICES all

# 필요한 패키지 설치 및 애플리케이션 설정
...

CMD ["python", "train.py"]

```

위 예시에서는 NVIDIA CUDA 11.0 기반 이미지를 사용하고, `CUDA_VISIBLE_DEVICES` 환경 변수를 `all`로 설정하여 모든 GPU를 컨테이너에 할당합니다.

## 결론

NVIDIA Docker를 사용하면 Docker 컨테이너 내에서 NVIDIA GPU를 활용할 수 있습니다. 이 문서에서는 NVIDIA Docker의 설치 방법과 사용 방법에 대해 설명했습니다. `--gpus` 옵션을 사용하여 GPU 자원을 할당하고, NVIDIA CUDA 이미지를 기반으로 컨테이너를 실행할 수 있습니다. 또한 Dockerfile에서 NVIDIA Docker를 사용하는 방법도 알아보았습니다. GPU 가속이 필요한 애플리케이션을 컨테이너화하여 실행할 때 NVIDIA Docker를 활용해보세요.