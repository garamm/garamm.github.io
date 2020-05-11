---
layout: book
---


ELK (with Docker)
---

### docker-compose 설치

```
curl -L https://github.com/docker/compose/releases/download/1.14.0/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose # 실행 권한 부여
```


### 프로젝트 다운로드

```
git clone https://github.com/deviantony/docker-elk.git
```

### elasticsearch.yml 수정

vi docker-elk/elasticsearch/config/elasticsearch.yml
```
## Default Elasticsearch configuration from Elasticsearch base image.
## https://github.com/elastic/elasticsearch/blob/master/distribution/docker/src/docker/config/elasticsearch.yml
#
cluster.name: "docker-cluster"
network.host: 0.0.0.0

## Use single node discovery in order to disable production mode and avoid bootstrap checks
## see https://www.elastic.co/guide/en/elasticsearch/reference/current/bootstrap-checks.html
#
discovery.type: single-node

#X-Pack관련 내용 주석 처리 (유료 라이선스이기 때문에 주석처리합니다.)
## X-Pack settings
## see https://www.elastic.co/guide/en/elasticsearch/reference/current/setup-xpack.html
#
#xpack.license.self_generated.type: trial
#xpack.security.enabled: true
#pack.monitoring.collection.enabled: true
```


### kibana.yml 수정
vi docker-elk/kibana/config/kibana.yml
```
## Default Kibana configuration from Kibana base image.
## https://github.com/elastic/kibana/blob/master/src/dev/build/tasks/os_packages/docker_generator/templates/kibana_yml.template.js
#elasticsearch.hosts를 서버 IP로 변경합니다.
server.name: kibana
server.host: "0.0.0.0"
elasticsearch.hosts: [ "http://elasticsearch server ip:9200" ]

#X-Pack관련 내용 주석 처리 (유료 라이선스이기 때문에 주석처리합니다.)
#xpack.monitoring.ui.container.elasticsearch.enabled: true

## X-Pack security credentials
#
#elasticsearch.username: elastic
#elasticsearch.password: changeme
```

### logstash.yml 파일 수정
vi docker-elk/logstash/config/logstash.yml
```
## Default Logstash configuration from Logstash base image.
## https://github.com/elastic/logstash/blob/master/docker/data/logstash/config/logstash-full.yml
#
http.host: "0.0.0.0"

#X-Pack관련 내용 주석 처리 (유료 라이선스이기 때문에 주석처리합니다.)
#xpack.monitoring.elasticsearch.hosts: [ "http://elasticsearch:9200" ]
## X-Pack security credentials
#xpack.monitoring.enabled: true
#xpack.monitoring.elasticsearch.username: elastic
#xpack.monitoring.elasticsearch.password: changeme
```

### logstash.conf 파일 수정
vi docker-elk/logstash/pipeline/logstash.conf
```
#filebeat 사용 선언 및 수신할 IP와 포트 지정
input {
  beats {
    port => 5000
    host => "0.0.0.0"
  }
}
#grok 형식으로 들어오는 로그를 가공하기 위해 필터 사용
# tags에 apache가 있을 경우 메시지 필드를 공용 아파치 로그로 변환하고 접속지의 IP를 기반으로 정보 가공 내용 추가하기
filter {
    if "apache" in [tags]{
        grok {
            match => { "message" => "%{COMMONAPACHELOG}" }
             }
        geoip {
            source => "clientip"
            target => "geoip"
              }
       }
}

# Logstash의 가공한 정보를 어디에 출력할지 설정
# 모든 데이터를 elk-%{+YYYY.MM.dd}라는 이름의 인덱스를 만들어서 Elasticsearch로 보내도록 설정
output {
        elasticsearch {
                hosts => "elasticsearch server ip:9200"
                index => "elk-%{+YYYY.MM.dd}"
        }
}
```

### docker-compose.yml 수정

```
version: '2'

services:

  elasticsearch:
    container_name:elasticsearch
    build:
      context: elasticsearch/
      args:
        ELK_VERSION: $ELK_VERSION
    volumes:
      - ./elasticsearch/config/elasticsearch.yml:/usr/share/elasticsearch/config/elasticsearch.yml:ro
      - /etc/localtime:/etc/localtime:ro
    ports:
      - "9200:9200"
      - "9300:9300"
    environment:
      ES_JAVA_OPTS: "-Xmx1024m -Xms1024m"
      #비밀번호는 사용하지 않기 때문에 주석처리
      #ELASTIC_PASSWORD: changeme
    networks:
      - elk

  logstash:
    container_name:logstash
    build:
      context: logstash/
      args:
        ELK_VERSION: $ELK_VERSION
    volumes:
      - ./logstash/config/logstash.yml:/usr/share/logstash/config/logstash.yml:ro
      - ./logstash/pipeline:/usr/share/logstash/pipeline:ro
      - /etc/localtime:/etc/localtime:ro
    ports:
      - "5000:5000"
      - "9600:9600"
    environment:
      LS_JAVA_OPTS: "-Xmx1024m -Xms1024m"
    networks:
      - elk
    depends_on:
      - elasticsearch

 kibana:
    container_name:kibana
    build:
      context: kibana/
      args:
        ELK_VERSION: $ELK_VERSION
    volumes:
      - ./kibana/config/kibana.yml:/usr/share/kibana/config/kibana.yml:ro
      - /etc/localtime:/etc/localtime:ro
    ports:
      - "5601:5601"
    # kibana 용량을 지정해준다
    environment:
      NODE_OPTIONS: "--max-old-space-size=2048"
    networks:
      - elk
    depends_on:
      - elasticsearch

networks:

  elk:
    driver: bridge
```


### docker-elk 폴더에 들어가서 docker-compose up 수행

```
cd docker-elk
docker-compose up -d
# -d : 백그라운드에서 실행 옵션)
```

### 모니터링 할 로그가 있는 서버에서 Filebeat 설치

```
curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.2.1-x86_64.rpm
rpm -vi filebeat-7.2.1-x86_64.rpm

# 우분투의 데비안 계열에서는 rpm패키지를 설치할 수 없어서 변환해야 함
apt-get install alien # alien 설치
alien -c filebeat-7.2.1-x86_64.rpm # 변환
dpkg -i filebeat_7.2.1-2_amd64.deb  # 설치
```

### filebeat.yml 수정

vi /etc/filebeat/filebeat.yml
```
  #=========================== Filebeat inputs =============================

  filebeat.inputs:

  # Each - is an input. Most options can be set at the input level, so
  # you can use different inputs for various configurations.
  # Below are the input specific configurations.

  - type: log

  # Change to true to enable this input configuration.
  enabled: true

  # Paths that should be crawled and fetched. Glob based paths.
  # 아파치 로그의 위치를 지정 *로 와일드카드 형식을 사용할 수 있음
  # Logstash에서 Tags를 이용하여 필터 규칙을 사용하기 때문에 사용
  paths:
    - /var/log/httpd/access_*
  tags : ["apache"]


  #============================== Kibana =====================================

  # Starting with Beats version 6.0.0, the dashboards are loaded via the Kibana API.
  # This requires a Kibana endpoint configuration.
  setup.kibana:

  # Kibana Host
  # Scheme and port can be left out and will be set to the default (http and 5601)
  # In case you specify and additional path, the scheme is required: http://localhost:5601/path
  # IPv6 addresses should always be defined as: https://[2001:db8::1]:5601
  # Kibana Server IP입력
  host: "Kibana Server IP:5601"

  # Elasticsearch로는 로그를 보내지 않을 것이기 때문에 다 주석처리
  #-------------------------- Elasticsearch output ------------------------------
  #output.elasticsearch:
  # Array of hosts to connect to.
  #hosts: ["0.0.0.0:9200"]

  #Optional protocol and basic auth credentials.
  #protocol: "https"
  #username: "elastic"
  #password: "changeme"

  #----------------------------- Logstash output --------------------------------
  output.logstash:
  # The Logstash hosts
  # Logstash Server IP 입력
  hosts: ["logstash server ip:5000"]

  #================================ Processors =====================================
  # 로그 파일을 보낼 때 서버의 Host정보와 Cloud정보를 보낼지 설정하는 부분
  # 필요 여부에 따라 주석 처리 하거나 하지 않습니다.
  # Configure processors to enhance or manipulate events generated by the beat.

  #processors:
  #  - add_host_metadata: ~
  #  - add_cloud_metadata: ~
```

### filebeat 실행

nohup filebeat -e -c /etc/filebeat/filebeat.yml &



참고<br>
https://setint.tistory.com/entry/%EC%9A%B0%EB%B6%84%ED%88%AC%EC%97%90%EC%84%9C-rpm-%EC%84%A4%EC%B9%98%EC%8B%9C-%EC%98%A4%EB%A5%98-%EB%82%A0-%EB%95%8C<br>
https://teichae.tistory.com/entry/Docker-Compose%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%98%EC%97%AC-ELK-Stack-%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0-1<br>
https://soye0n.tistory.com/173<br>
https://discuss.elastic.co/t/kibana-server-is-not-ready-yet-kibana-7-6-2-new-install/227840/6<br>



