# Prometheus와 Grafana를 활용한 메트릭 수집 및 시각화

# 11.3. Prometheus와 Grafana를 활용한 메트릭 수집 및 시각화

서버 모니터링 및 관리를 위해서는 시스템 메트릭을 수집하고 시각화하는 것이 중요합니다. Prometheus와 Grafana는 강력하고 유연한 오픈 소스 모니터링 솔루션으로, 시스템 메트릭을 수집하고 시각화하는 데 널리 사용됩니다. 이 문서에서는 Prometheus와 Grafana를 활용하여 메트릭을 수집하고 시각화하는 방법에 대해 자세히 설명하겠습니다.

## Prometheus 개요

Prometheus는 시계열 데이터베이스와 모니터링 시스템으로, 메트릭을 수집하고 저장하며 알림을 생성할 수 있습니다. 주요 특징은 다음과 같습니다:

- 시계열 데이터 수집 및 저장
- 유연한 쿼리 언어 (PromQL)
- 다차원 데이터 모델
- 풀링 방식의 메트릭 수집
- 다양한 시각화 및 대시보드 옵션
- 알림 및 경고 기능

## Prometheus 설치 및 구성

1. Prometheus 다운로드:
    
    ```
    wget <https://github.com/prometheus/prometheus/releases/download/v2.30.0/prometheus-2.30.0.linux-amd64.tar.gz>
    tar xvf prometheus-2.30.0.linux-amd64.tar.gz
    cd prometheus-2.30.0.linux-amd64
    
    ```
    
2. Prometheus 구성 파일 (`prometheus.yml`) 수정:
    
    ```yaml
    global:
      scrape_interval: 15s
    
    scrape_configs:
      - job_name: 'prometheus'
        static_configs:
          - targets: ['localhost:9090']
    
      - job_name: 'node_exporter'
        static_configs:
          - targets: ['localhost:9100']
    
    ```
    
    위 구성 파일에서는 Prometheus 자체 메트릭(`localhost:9090`)과 Node Exporter 메트릭(`localhost:9100`)을 수집하도록 설정되어 있습니다.
    
3. Prometheus 실행:
    
    ```
    ./prometheus --config.file=prometheus.yml
    
    ```
    
4. 웹 브라우저에서 `http://서버_IP:9090`에 접속하여 Prometheus 웹 인터페이스에 액세스할 수 있습니다.

## Node Exporter 설치 및 구성

Node Exporter는 호스트 시스템의 메트릭을 수집하여 Prometheus에 제공하는 에이전트입니다.

1. Node Exporter 다운로드:
    
    ```
    wget <https://github.com/prometheus/node_exporter/releases/download/v1.2.2/node_exporter-1.2.2.linux-amd64.tar.gz>
    tar xvf node_exporter-1.2.2.linux-amd64.tar.gz
    cd node_exporter-1.2.2.linux-amd64
    
    ```
    
2. Node Exporter 실행:
    
    ```
    ./node_exporter
    
    ```
    
3. Node Exporter는 기본적으로 9100 포트에서 실행되며, 시스템 메트릭을 수집하여 Prometheus에 제공합니다.

## Grafana 개요

Grafana는 데이터 시각화 및 모니터링 플랫폼으로, 다양한 데이터 소스에서 데이터를 가져와 대시보드와 그래프를 생성할 수 있습니다. Prometheus와 함께 사용하여 수집된 메트릭을 시각화하고 분석할 수 있습니다.

## Grafana 설치 및 구성

1. Grafana 설치:
    
    ```
    sudo apt-get update
    sudo apt-get install -y apt-transport-https
    sudo apt-get install -y software-properties-common wget
    wget -q -O - <https://packages.grafana.com/gpg.key> | sudo apt-key add -
    echo "deb <https://packages.grafana.com/oss/deb> stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
    sudo apt-get update
    sudo apt-get install grafana
    
    ```
    
2. Grafana 서비스 시작:
    
    ```
    sudo systemctl start grafana-server
    sudo systemctl enable grafana-server
    
    ```
    
3. 웹 브라우저에서 `http://서버_IP:3000`에 접속하여 Grafana 웹 인터페이스에 액세스할 수 있습니다. 기본 사용자 이름과 비밀번호는 `admin/admin`입니다.

## Grafana에서 Prometheus 데이터 소스 추가

1. Grafana 웹 인터페이스에 로그인합니다.
2. 왼쪽 메뉴에서 `Configuration` -> `Data Sources`를 클릭합니다.
3. `Add data source` 버튼을 클릭하고 `Prometheus`를 선택합니다.
4. Prometheus의 URL(`http://서버_IP:9090`)을 입력하고 `Save & Test` 버튼을 클릭합니다.

## 대시보드 생성 및 메트릭 시각화

1. Grafana 웹 인터페이스에서 왼쪽 메뉴의 `+` 버튼을 클릭하고 `Dashboard`를 선택합니다.
2. `Add new panel`을 클릭하여 새로운 패널을 추가합니다.
3. 패널 편집 모드에서 `Query` 탭을 선택하고 Prometheus 쿼리를 입력합니다. 예를 들어, CPU 사용량을 시각화하려면 `node_cpu_seconds_total` 메트릭을 사용할 수 있습니다.
4. 패널의 제목, 축 레이블, 범례 등을 설정하고 `Apply` 버튼을 클릭하여 변경 사항을 저장합니다.
5. 다양한 메트릭에 대한 패널을 추가하고 구성하여 대시보드를 완성할 수 있습니다.

## 알림 및 경고 설정

Grafana에서는 메트릭 임계값을 기반으로 알림 및 경고를 설정할 수 있습니다.

1. 대시보드에서 패널을 선택하고 `Edit` 버튼을 클릭합니다.
2. `Alert` 탭을 선택하고 `Create Alert` 버튼을 클릭합니다.
3. 알림 조건, 임계값, 평가 기간 등을 설정하고 알림 채널(이메일, Slack 등)을 선택합니다.
4. `Save` 버튼을 클릭하여 알림을 저장합니다.

이제 설정된 조건에 따라 알림이 트리거되며, 지정된 채널로 알림이 전송됩니다.

## 결론

이 문서에서는 Prometheus와 Grafana를 활용하여 메트릭을 수집하고 시각화하는 방법에 대해 설명했습니다. Prometheus를 사용하여 시계열 데이터를 수집하고 저장하며, Node Exporter를 통해 호스트 시스템의 메트릭을 수집할 수 있습니다. Grafana를 사용하여 Prometheus의 데이터를 시각화하고 대시보드를 생성할 수 있으며, 알림 및 경고 기능을 통해 문제를 신속하게 감지하고 대응할 수 있습니다. Prometheus와 Grafana를 함께 사용함으로써 효과적인 모니터링 시스템을 구축하고 서버 성능을 최적화할 수 있습니다.