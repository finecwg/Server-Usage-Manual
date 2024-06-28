# R

# 6.2. R

이 문서에서는 Ubuntu 서버에서 R 언어 개발 환경을 구성하고, 데이터 분석, 시각화 및 Bioconductor를 활용한 생명과학 데이터 분석 방법에 대해 자세히 설명합니다. R은 통계 계산 및 그래픽을 위한 오픈소스 프로그래밍 언어이자 소프트웨어 환경으로, 데이터 과학자와 연구자들 사이에서 널리 사용되고 있습니다.

## 6.2.1. R 설치

### 6.2.1.1. R 패키지 설치

- R 설치를 위한 저장소 추가:
    
    ```bash
    sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9
    sudo add-apt-repository 'deb <https://cloud.r-project.org/bin/linux/ubuntu> jammy-cran40/'
    
    ```
    
- R 패키지 설치:
    
    ```bash
    sudo apt update
    sudo apt install r-base
    
    ```
    

Ubuntu 서버에서 R을 설치하려면 먼저 R 프로젝트 저장소를 시스템에 추가해야 합니다. `apt-key` 및 `add-apt-repository` 명령을 사용하여 저장소를 추가한 후, `apt` 명령을 사용하여 R 패키지를 설치할 수 있습니다.

### 6.2.1.2. RStudio Server 설치

- RStudio Server 설치:
    
    ```bash
    sudo apt install gdebi-core
    wget <https://download2.rstudio.org/server/jammy/amd64/rstudio-server-2023.03.0-386-amd64.deb>
    sudo gdebi rstudio-server-2023.03.0-386-amd64.deb
    
    ```
    

RStudio Server는 웹 브라우저를 통해 액세스할 수 있는 R용 통합 개발 환경(IDE)입니다. `gdebi-core` 패키지를 설치하고, RStudio Server의 `.deb` 패키지를 다운로드한 후, `gdebi` 명령을 사용하여 패키지를 설치할 수 있습니다.

## 6.2.2. R 패키지 관리

### 6.2.2.1. CRAN을 통한 패키지 설치

- R 콘솔에서 패키지 설치:
    
    ```
    install.packages("패키지명")
    
    ```
    
- 예시:
    
    ```
    install.packages("ggplot2")
    
    ```
    

CRAN(Comprehensive R Archive Network)은 R 패키지의 중앙 저장소입니다. R 콘솔에서 `install.packages()` 함수를 사용하여 CRAN에서 패키지를 설치할 수 있습니다.

### 6.2.2.2. Github을 통한 패키지 설치

- `devtools` 패키지 설치:
    
    ```
    install.packages("devtools")
    
    ```
    
- Github에서 패키지 설치:
    
    ```
    devtools::install_github("사용자명/저장소명")
    
    ```
    
- 예시:
    
    ```
    devtools::install_github("tidyverse/dplyr")
    
    ```
    

Github에 호스팅된 R 패키지를 설치하려면 `devtools` 패키지가 필요합니다. `install_github()` 함수를 사용하여 Github 저장소에서 패키지를 직접 설치할 수 있습니다.

## 6.2.3. 데이터 조작 및 시각화

### 6.2.3.1. dplyr 패키지를 활용한 데이터 조작

- `dplyr` 패키지 설치 및 로드:
    
    ```
    install.packages("dplyr")
    library(dplyr)
    
    ```
    
- `dplyr` 주요 함수:
    - `filter()`: 조건에 맞는 행 추출
    - `select()`: 열 선택
    - `mutate()`: 새로운 변수 생성
    - `group_by()`, `summarize()`: 그룹화 및 요약 통계량 계산
- 예시:
    
    ```
    data %>%
      filter(column1 > 10) %>%
      select(column2, column3) %>%
      mutate(new_column = column2 / column3) %>%
      group_by(column4) %>%
      summarize(mean_value = mean(new_column))
    
    ```
    

`dplyr` 패키지는 데이터 조작을 위한 강력하고 직관적인 문법을 제공합니다. `%>%` 파이프 연산자를 사용하여 데이터 처리 단계를 연결할 수 있으며, 각 단계에서 필요한 함수를 사용하여 데이터를 변환할 수 있습니다.

### 6.2.3.2. ggplot2 패키지를 활용한 데이터 시각화

- `ggplot2` 패키지 설치 및 로드:
    
    ```
    install.packages("ggplot2")
    library(ggplot2)
    
    ```
    
- `ggplot2` 기본 문법:
    
    ```
    ggplot(data, aes(x = x변수, y = y변수)) +
      geom_점 또는 선 종류(매핑 = 값, 옵션 = 값) +
      추가 레이어 +
      테마 및 기타 옵션
    
    ```
    
- 예시:
    
    ```
    ggplot(iris, aes(x = Sepal.Length, y = Sepal.Width, color = Species)) +
      geom_point(size = 3, alpha = 0.7) +
      labs(title = "Iris Data Scatterplot", x = "Sepal Length", y = "Sepal Width") +
      theme_minimal()
    
    ```
    

`ggplot2` 패키지는 R에서 가장 널리 사용되는 데이터 시각화 패키지 중 하나입니다. `ggplot()` 함수를 사용하여 그래프의 기본 구조를 설정하고, `geom_` 함수를 사용하여 실제 데이터를 표현하는 레이어를 추가합니다. 다양한 옵션과 테마를 적용하여 그래프를 사용자 정의할 수 있습니다.

## 6.2.4. Bioconductor를 활용한 생명과학 데이터 분석

### 6.2.4.1. Bioconductor 설치

- Bioconductor 설치:
    
    ```
    install.packages("BiocManager")
    BiocManager::install()
    
    ```
    

Bioconductor는 R을 위한 오픈소스 소프트웨어 프로젝트로, 생물정보학 및 생명과학 데이터 분석을 위한 도구와 알고리즘을 제공합니다. `BiocManager` 패키지를 설치하고 `BiocManager::install()` 함수를 사용하여 Bioconductor를 설치할 수 있습니다.

### 6.2.4.2. Bioconductor 패키지 설치 및 활용

- Bioconductor 패키지 설치:
    
    ```
    BiocManager::install("패키지명")
    
    ```
    
- 예시: `DESeq2` 패키지 설치 및 로드
    
    ```
    BiocManager::install("DESeq2")
    library(DESeq2)
    
    ```
    
- `DESeq2`를 활용한 RNA-seq 데이터 분석 예시:
    
    ```
    dds <- DESeqDataSetFromMatrix(countData = counts, colData = samples, design = ~ condition)
    dds <- DESeq(dds)
    res <- results(dds)
    significant_genes <- subset(res, padj < 0.05 & abs(log2FoldChange) > 1)
    
    ```
    

Bioconductor는 다양한 생물정보학 분석 패키지를 제공합니다. 패키지 설치는 `BiocManager::install()` 함수를 사용하여 수행할 수 있습니다. 예를 들어, `DESeq2` 패키지를 사용하여 RNA-seq 데이터의 차등 발현 분석을 수행할 수 있습니다. `DESeqDataSetFromMatrix()` 함수로 데이터를 가져오고, `DESeq()` 함수로 모델을 적합한 후, `results()` 함수로 결과를 추출할 수 있습니다.

이 문서에서는 Ubuntu 서버에서 R 언어 개발 환경을 구성하고, R을 활용한 데이터 분석, 시각화 및 Bioconductor를 활용한 생명과학 데이터 분석 방법에 대해 자세히 다루었습니다. RStudio Server를 설치하여 웹 기반 IDE를 제공하고, CRAN 및 Github을 통해 다양한 패키지를 설치하고 관리할 수 있습니다. `dplyr`와 `ggplot2` 패키지를 활용하여 데이터 조작과 시각화를 수행하고, Bioconductor 프로젝트의 패키지를 활용하여 생명과학 데이터 분석을 진행할 수 있습니다. 이 문서를 참조하여 R을 활용한 데이터 과학 및 생물정보학 프로젝트를 성공적으로 수행할 수 있을 것입니다.