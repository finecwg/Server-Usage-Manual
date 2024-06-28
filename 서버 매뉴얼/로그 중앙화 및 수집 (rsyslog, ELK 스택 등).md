# 로그 중앙화 및 수집 (rsyslog, ELK 스택 등)

# 11.2. 로그 중앙화 및 수집 (rsyslog, ELK 스택 등)

서버 로그는 시스템의 동작을 이해하고 문제를 진단하는 데 중요한 역할을 합니다. 여러 대의 서버에서 생성되는 로그를 중앙화하여 수집 및 관리하면 로그 분석과 모니터링이 효율적으로 이루어질 수 있습니다. 이 문서에서는 로그 중앙화 및 수집을 위해 사용되는 도구인 rsyslog와 ELK 스택에 대해 자세히 설명하겠습니다.

## rsyslog

rsyslog는 시스템 로그 메시지를 수집, 필터링, 저장 및 전달하기 위한 오픈 소스 로그 관리 도구입니다. 기존의 syslog 데몬을 대체하며, 더 많은 기능과 유연성을 제공합니다. rsyslog를 사용하여 여러 서버의 로그를 중앙 서버로 전송하고 관리할 수 있습니다.

### rsyslog 서버 설정

1. rsyslog 설치:
    
    ```
    sudo apt-get update
    sudo apt-get install rsyslog
    
    ```
    
2. rsyslog 설정 파일 수정:
    
    `/etc/rsyslog.conf` 파일을 열고 다음 내용을 추가합니다:
    
    ```
    # 로그 수신을 위한 UDP 포트 설정
    module(load="imudp")
    input(type="imudp" port="514")
    
    # 로그 수신을 위한 TCP 포트 설정
    module(load="imtcp")
    input(type="imtcp" port="514")
    
    # 수신된 로그 파일 저장 디렉토리 설정
    $template RemoteServer, "/var/log/remote/%HOSTNAME%/%$YEAR%-%$MONTH%-%$DAY%.log"
    *.* ?RemoteServer
    
    ```
    
    위 설정은 UDP 및 TCP 514 포트로 로그를 수신하고, 수신된 로그를 `/var/log/remote/원격서버이름/년-월-일.log` 형식으로 저장합니다.
    
3. rsyslog 재시작:
    
    ```
    sudo systemctl restart rsyslog
    
    ```
    

### 클라이언트 서버 설정

1. rsyslog 설치:
    
    ```
    sudo apt-get update
    sudo apt-get install rsyslog
    
    ```
    
2. rsyslog 설정 파일 수정:
    
    `/etc/rsyslog.conf` 파일을 열고 다음 내용을 추가합니다:
    
    ```
    # 로그를 중앙 서버로 전송
    *.* @중앙서버IP:514
    
    ```
    
    위 설정은 모든 로그 메시지를 중앙 서버의 514 포트로 전송합니다.
    
3. rsyslog 재시작:
    
    ```
    sudo systemctl restart rsyslog
    
    ```
    

### rsyslog 로그 로테이션 설정

중앙 서버에서 수집된 로그 파일의 크기가 커질 경우 로그 로테이션을 설정하여 관리할 수 있습니다. `/etc/logrotate.d/rsyslog` 파일을 생성하고 다음 내용을 추가합니다:

```
/var/log/remote/*/*.log {
    daily
    rotate 7
    compress
    delaycompress
    missingok
    notifempty
    sharedscripts
    postrotate
        /bin/kill -HUP `cat /var/run/rsyslogd.pid 2>/dev/null` 2>/dev/null || true
    endscript
}

```

위 설정은 `/var/log/remote` 디렉토리 하위의 모든 로그 파일을 매일 로테이션하고, 7개의 로그 파일을 유지하며, 압축하여 저장합니다.

## ELK 스택

ELK 스택은 Elasticsearch, Logstash, Kibana의 조합으로, 로그 수집, 검색, 분석 및 시각화를 위한 강력한 도구 모음입니다. ELK 스택을 사용하면 대규모 로그 데이터를 효과적으로 관리하고 실시간으로 분석할 수 있습니다.

### Elasticsearch 설치 및 설정

1. Elasticsearch 설치:
    
    ```
    wget -qO - <https://artifacts.elastic.co/GPG-KEY-elasticsearch> | sudo apt-key add -
    echo "deb <https://artifacts.elastic.co/packages/7.x/apt> stable main" | sudo tee /etc/apt/sources.list.d/elastic-7.x.list
    sudo apt-get update
    sudo apt-get install elasticsearch
    
    ```
    
2. Elasticsearch 설정 파일 수정:
    
    `/etc/elasticsearch/elasticsearch.yml` 파일을 열고 다음 내용을 수정합니다:
    
    ```
    network.host: 0.0.0.0
    discovery.seed_hosts: ["localhost"]
    
    ```
    
3. Elasticsearch 서비스 시작:
    
    ```
    sudo systemctl start elasticsearch
    sudo systemctl enable elasticsearch
    
    ```
    

### Logstash 설치 및 설정

1. Logstash 설치:
    
    ```
    sudo apt-get install logstash
    
    ```
    
2. Logstash 설정 파일 생성:
    
    `/etc/logstash/conf.d/logstash.conf` 파일을 생성하고 다음 내용을 추가합니다:
    
    ```
    input {
      beats {
        port => 5044
      }
    }
    
    output {
      elasticsearch {
        hosts => ["localhost:9200"]
        index => "%{[@metadata][beat]}-%{+YYYY.MM.dd}"
      }
    }
    
    ```
    
3. Logstash 서비스 시작:
    
    ```
    sudo systemctl start logstash
    sudo systemctl enable logstash
    
    ```
    

### Kibana 설치 및 설정

1. Kibana 설치:
    
    ```
    sudo apt-get install kibana
    
    ```
    
2. Kibana 설정 파일 수정:
    
    `/etc/kibana/kibana.yml` 파일을 열고 다음 내용을 수정합니다:
    
    ```
    server.host: "0.0.0.0"
    elasticsearch.hosts: ["<http://localhost:9200>"]
    
    ```
    
3. Kibana 서비스 시작:
    
    ```
    sudo systemctl start kibana
    sudo systemctl enable kibana
    
    ```
    
4. 웹 브라우저에서 `http://서버_IP:5601`에 접속하여 Kibana 웹 인터페이스에 액세스할 수 있습니다.

### Filebeat를 사용하여 로그 전송

Filebeat는 경량의 로그 수집기로, 서버의 로그 파일을 읽어 Logstash 또는 Elasticsearch로 전송합니다.

1. Filebeat 설치:
    
    ```
    sudo apt-get install filebeat
    
    ```
    
2. Filebeat 설정 파일 수정:
    
    `/etc/filebeat/filebeat.yml` 파일을 열고 다음 내용을 수정합니다:
    
    ```
    filebeat.inputs:
    - type: log
      paths:
        - /var/log/*.log
    
    output.logstash:
      hosts: ["localhost:5044"]
    
    ```
    
3. Filebeat 서비스 시작:
    
    ```
    sudo systemctl start filebeat
    sudo systemctl enable filebeat
    
    ```
    

위 설정은 `/var/log` 디렉토리의 모든 `.log` 파일을 읽어 Logstash의 5044 포트로 전송합니다.

## 결론

이 문서에서는 로그 중앙화 및 수집을 위한 도구인 rsyslog와 ELK 스택에 대해 알아보았습니다. rsyslog를 사용하여 여러 서버의 로그를 중앙 서버로 전송하고 관리할 수 있으며, ELK 스택을 통해 대규모 로그 데이터를 수집, 검색, 분석 및 시각화할 수 있습니다. 로그 중앙화 및 수집을 통해 서버 로그를 효과적으로 관리하고 모니터링할 수 있으며, 이를 통해 시스템 문제를 신속하게 파악하고 대응할 수 있습니다. 서버 관리자는 rsyslog와 ELK 스택을 활용하여 로그 관리 체계를 구축함으로써 안정적이고 효율적인 서버 운영을 할 수 있습니다.