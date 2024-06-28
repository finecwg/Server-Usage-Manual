# Singularity

# 10.3. Singularity

Singularity는 고성능 컴퓨팅(HPC) 환경에서 사용되는 컨테이너 플랫폼으로, 보안성과 이식성을 강조합니다. 이 문서에서는 Singularity의 기본 개념, 설치 방법, 사용 방법 및 활용 사례에 대해 자세히 설명하겠습니다.

## Singularity 개요

Singularity는 Docker와 유사한 컨테이너 기술이지만, HPC 환경에 최적화되어 있습니다. 주요 특징은 다음과 같습니다:

- 사용자 권한: Singularity 컨테이너는 사용자 권한으로 실행되므로 루트 권한이 필요하지 않습니다.
- 파일 시스템 액세스: 호스트 시스템의 파일 시스템을 컨테이너 내부에서 직접 액세스할 수 있습니다.
- GPU 및 인터커넥트 지원: Singularity는 GPU와 고성능 인터커넥트(예: InfiniBand)를 지원합니다.
- 이식성: Singularity 컨테이너는 다양한 Linux 배포판과 HPC 시스템에서 실행할 수 있습니다.

## Singularity 설치

Singularity를 설치하려면 다음 단계를 따르세요:

1. 필요한 종속성 설치:
    
    ```
    sudo apt-get update
    sudo apt-get install -y build-essential libssl-dev uuid-dev libgpgme11-dev squashfs-tools libseccomp-dev wget pkg-config git cryptsetup
    
    ```
    
2. Go 언어 설치:
    
    ```
    export VERSION=1.17.2 OS=linux ARCH=amd64
    wget <https://dl.google.com/go/go$VERSION.$OS-$ARCH.tar.gz>
    sudo tar -C /usr/local -xzvf go$VERSION.$OS-$ARCH.tar.gz
    rm go$VERSION.$OS-$ARCH.tar.gz
    echo 'export PATH=/usr/local/go/bin:$PATH' >> ~/.bashrc
    source ~/.bashrc
    
    ```
    
3. Singularity 소스 코드 다운로드 및 빌드:
    
    ```
    export VERSION=3.8.0
    wget <https://github.com/hpcng/singularity/releases/download/v${VERSION}/singularity-${VERSION}.tar.gz>
    tar -xzf singularity-${VERSION}.tar.gz
    cd singularity
    ./mconfig
    make -C builddir
    sudo make -C builddir install
    
    ```
    
4. 설치 확인:
    
    ```
    singularity --version
    
    ```
    

## Singularity 컨테이너 생성

Singularity 컨테이너를 생성하는 방법에는 두 가지가 있습니다: 정의 파일을 사용하는 방법과 기존 컨테이너 이미지를 사용하는 방법입니다.

### 정의 파일을 사용한 컨테이너 생성

1. 정의 파일(`example.def`) 작성:
    
    ```
    Bootstrap: docker
    From: ubuntu:20.04
    
    %post
        apt-get update
        apt-get install -y Python3
    
    %runscript
        Python3 "$@"
    
    ```
    
2. 정의 파일에서 컨테이너 이미지 빌드:
    
    ```
    sudo singularity build example.sif example.def
    
    ```
    

### 기존 컨테이너 이미지 사용

1. Singularity 컨테이너 이미지 다운로드:
    
    ```
    singularity pull docker://ubuntu:20.04
    
    ```
    
2. 컨테이너 이미지를 SIF 파일로 변환:
    
    ```
    singularity build ubuntu_20.04.sif docker://ubuntu:20.04
    
    ```
    

## Singularity 컨테이너 실행

Singularity 컨테이너를 실행하는 방법은 다음과 같습니다:

1. 대화형 셸로 컨테이너 실행:
    
    ```
    singularity shell example.sif
    
    ```
    
2. 컨테이너 내부에서 명령 실행:
    
    ```
    singularity exec example.sif Python3 --version
    
    ```
    
3. 컨테이너의 기본 런스크립트 실행:
    
    ```
    singularity run example.sif
    
    ```
    

## HPC 환경에서 Singularity 활용

Singularity는 HPC 환경에서 많이 사용되며, 다음과 같은 방식으로 활용할 수 있습니다:

1. MPI 작업 실행:
    
    ```
    mpirun -np 4 singularity exec example.sif mpi_program
    
    ```
    
2. GPU 활용:
    
    ```
    singularity exec --nv example.sif Python3 gpu_script.py
    
    ```
    
3. 병렬 파일 시스템 마운트:
    
    ```
    singularity exec --bind /scratch:/data example.sif process_data.py
    
    ```
    

## 결론

이 문서에서는 Singularity 컨테이너 플랫폼의 기본 개념, 설치 방법, 사용 방법 및 HPC 환경에서의 활용 방안에 대해 설명했습니다. Singularity는 보안성과 이식성을 중요시하는 HPC 환경에 최적화된 컨테이너 솔루션입니다. 정의 파일을 사용하거나 기존 컨테이너 이미지를 활용하여 Singularity 컨테이너를 생성할 수 있으며, 다양한 방식으로 컨테이너를 실행할 수 있습니다. 또한 MPI 작업, GPU 활용, 병렬 파일 시스템 마운트 등 HPC 환경에서 Singularity를 효과적으로 활용할 수 있습니다. 이 문서의 내용을 참고하여 Singularity를 설치하고 HPC 환경에서 컨테이너를 활용해보세요.