---
description: ELK 로그쌓고 알람보내기 따라하기
---

# ELK 로그쌓고 알람보내기 따라하기

안녕하세요 평범한 개발자 김봉준입니다.

{% hint style="info" %}
해당 포스팅 내용은 docker-elk로 간단하게 만들어보는 ELK 로그 쌓기 튜토리얼 입니다.
{% endhint %}

로그 쌓고 키바나로 보고 키바나 알람 플러그인 하나 설치해서 슬랙으로 받는것 까지 대략적인 샘플입니다.

{% hint style="success" %}
아래 내용은 우형 기술블로그에 나와있길래 따라하면서 제가 했던 프로젝트에 적용을 로컬기준으로만 해본 내용이에요. \( 튜토리얼 수준 \)

* 참고로 저는 도커 잘몰라요. , ELK도 해당 샘플 만들었던 2020.1.31일 처음 해봤습니다. 
* 부족한 내용은 너그럽게 넘어가 주시고 , 잘아시는분들은 조언 부탁드립니다.
{% endhint %}

일단 사용될 es logstash , kibana 를 받아볼까요  git hub에서 docker-elk 를 받아줍니다. 

```text
// git hub 주소 :  https://github.com/deviantony/docker-elk
git clone  https://github.com/deviantony/docker-elk.git
```

여기서 클론받은 부분을 docker-compose 로 바로 실행할경우 메모리나 기타 에러를 만날 수 있어요

그래서 실행하기 전에 일단 조금 설정 변경하고 키면 됩니다. 

첫번째로 변경해야 할 파일은 로그스태시 설정 파일을 조금 변경해 줍니다. \( logstash.conf \)

```text
-rw-r--r--  1 amore  staff   256B  1 30 13:44 logstash.conf
➜  pipeline (master) ✗ pwd
/Users/amore/docker/docker-elk/logstash/pipeline
 
 
// logstash.conf 파일 내용
input {
        tcp {
                port => 5000
                codec => json_lines
        }
}
 
## Add your filters / logstash plugins configuration here
 
output {
        elasticsearch {
                hosts => "elasticsearch:9200"
                index => "logstash-20200130" // index명 추가 (정해진 규칙이 있다면 규칙대로 하셔도 됩니다.)
                user => "elastic"
                password => "changeme"
        }
}
```

이제 다음으로는 ELK 에 open distro alert 를 설치해 줍니다.

```text
-rw-r--r--  1 amore  staff   394B  1 30 14:03 Dockerfile
drwxr-xr-x  3 amore  staff    96B  1 31 11:10 config
➜  elasticsearch (master) ✗ pwd
/Users/amore/docker/docker-elk/elasticsearch
 
 
// Dockerfile에 다음과 같은 RUN 부분을 한줄을 추가해 줍니다.
 
 
ARG ELK_VERSION
FROM docker.elastic.co/elasticsearch/elasticsearch:${ELK_VERSION
RUN elasticsearch-plugin install -b https://d3g5vo6xdbdb9a.cloudfront.net/downloads/elasticsearch-plugins/opendistro-alerting/opendistro_alerting-1.3.0.1.zip
```

추가적으로 xpack 관련 설정을 false 로 변경해줍니다. \( opendistro\_alert 사용할때 true 일 경우 에러가 난다고 합니다. \)

```text
-rw-r--r--  1 amore  staff   538B  1 30 14:06 elasticsearch.yml
➜  config (master) ✗ pwd
/Users/amore/docker/docker-elk/elasticsearch/config
 
 
// elasticsearch.yml 파일 설정 추가.
 
 
xpack.security.enabled: false
xpack.monitoring.collection.enabled: false
```

이제 도커 이미지를 빌드하고 컨테이너를 실행해 봅니다. 

실행하기 앞아서 docker-compose.yml 에 메모리 설정을 조금 건드려 줍니다. 

elk 는 512 로 설정, logstash도  512로 설정 kibana 는 2기가로 설정   \( 전체 설정부분 아래 참조 \) 

```text
version: '3.2'
 
services:
  elasticsearch:
    build:
      context: elasticsearch/
      args:
        ELK_VERSION: $ELK_VERSION
    volumes:
      - type: bind
        source: ./elasticsearch/config/elasticsearch.yml
        target: /usr/share/elasticsearch/config/elasticsearch.yml
        read_only: true
      - type: volume
        source: elasticsearch
        target: /usr/share/elasticsearch/data
    ports:
      - "9200:9200"
      - "9300:9300"
    environment:
      ES_JAVA_OPTS: "-Xmx512m -Xms512m"
      ELASTIC_PASSWORD: changeme
      # Use single node discovery in order to disable production mode and avoid bootstrap checks
      # see https://www.elastic.co/guide/en/elasticsearch/reference/current/bootstrap-checks.html
      discovery.type: single-node
    networks:
      - elk
 
  logstash:
    build:
      context: logstash/
      args:
        ELK_VERSION: $ELK_VERSION
    volumes:
      - type: bind
        source: ./logstash/config/logstash.yml
        target: /usr/share/logstash/config/logstash.yml
        read_only: true
      - type: bind
        source: ./logstash/pipeline
        target: /usr/share/logstash/pipeline
        read_only: true
    ports:
      - "5000:5000"
      - "9600:9600"
    environment:
      LS_JAVA_OPTS: "-Xmx512m -Xms512m"
    networks:
      - elk
    depends_on:
      - elasticsearch
 
 
  kibana:
    build:
      context: kibana/
      args:
        ELK_VERSION: $ELK_VERSION
    volumes:
      - type: bind
        source: ./kibana/config/kibana.yml
        target: /usr/share/kibana/config/kibana.yml
        read_only: true
    ports:
      - "5601:5601"
    environment:
      NODE_OPTIONS: "--max-old-space-size=2048"
    networks:
      - elk
    depends_on:
      - elasticsearch
 
networks:
  elk:
    driver: bridge
 
volumes:
  elasticsearch:
```

네 이제 기본 설정 끝났습니다. 그냥 이제 실행 해보면 키바나를 확인할 수 있을꺼에요.

```text
docker-compose build
docker volume rm docker-elk_elasticsearch
docker-compose up
```

![&#xB3C4;&#xCEE4;&#xC2E4;&#xD589;&#xD654;&#xBA74;](https://curtiskim.github.io/img/elk/100.png)

키바나 화면

![&#xD0A4;&#xBC14;&#xB098;&#xD654;&#xBA74;](https://curtiskim.github.io/img/elk/101.png)

* 스크린샷에 로그가 이미 쌓여있는데요. 이부분은 제가 샘플 만들면서 샘플로 쌓아봤던 내용입니다.

여기서 이제 로그 스테시로 우리가 만든 어플리케이션이 어떻게 로그를 쌓을까 ? 하는 부분이 있는데요 이부분도 어플리케이션 로그 쌓는 부분을 변경 해줘야합니다. 저는 기존 프로젝트를 샘플로 삼았는데요. 

해등 프로젝트는 spring boot 로 간단하게 제작된 웹 어플리케이션 입니다.   log 쌓는 부분은 logback 에 위임해서 쌓고있어요. 

지금은 console appender 와 ,  file rolling appender 를 사용해서, 파일로 쌓고 콘솔로 남기고 있습니다.  여기에 이제 우리는  logback logstash  encoder 를 추가해주면 됩니다. 

우선 적으로 해당 플젝트는 gradle을 사용하고있어요 그래서 이제 의존성 관리하는 build.gradle 파일에 다음과 같이 추가 해줍니다. 

```text
implementation 'net.logstash.logback:logstash-logback-encoder:6.3'
```

log level 설정은 각 어플리케이션에 알맞게 설정된 부분으로 그대로 사용하시면 됩니다. 

해당 의존성을 추가 해줬다면, log back 설정 부분을 조금 변경해 줍니다.  

```text
...
 
 
<appender name="stash" class="net.logstash.logback.appender.LogstashTcpSocketAppender">
  <destination>127.0.0.1:5000</destination>
  <!-- encoder is required -->
  <encoder class="net.logstash.logback.encoder.LogstashEncoder" />
</appender>
 
 
...
```

* 여기서 추가적으로 해당 tcpsockerappender 에도 로그 레벨 필터를 적용해서 로그스태시로 쌓는 부분의 로그레벨을 정리할수있습니다.
* 또한 아마 패턴도 따로 정리가 가능할 것 같은데요 이부분은 공식 사이트 가이드를 좀더 확인해봐야 될 것 같아요.

```text
...
 
 
  <filter class="ch.qos.logback.classic.filter.LevelFilter">
    <level>INFO</level>
        <onMatch>ACCEPT</onMatch>
        <onMismatch>DENY</onMismatch>
  </filter>
 
 
...
```

위와 같이 설정을 해준후에 spring Profile 설정을 appender-ref 를 이제 위와 같이 만들어준 appender 를 지정을 해줘야합니다. \( logback 이 익숙하지 않은 분들은 logback 공홈에서 확인 부탁드립니다. \)

```text
<springProfile name="local">
  <logger name="com.ap" level="DEBUG"/>
  <logger name="org.springframework.boot.devtools" level="DEBUG"/>
 
  <root level="INFO">
    <appender-ref ref="STDOUT" />
    <appender-ref ref="stash"/>
  </root>
</springProfile>
```

이제 어플리케이션 로그를 ELK 로 쌓는 부분은 끝이 났습니다. 이제 여기서 에러 알람만 가지고 슬랙으로 보내는부분을 해볼텐데요 . \(아까 ELK 에 알랏팅 플러그인  \) 

키바나에 접속을 하여 알랏기능 설저을 해주면 됩니다.  \( 알랏 메뉴에서 destination 을 추가해줍니다. \) 

![&#xC54C;&#xB78F;&#xC124;&#xC815;1](https://curtiskim.github.io/img/elk/102.png)

* 여기서 webhook url 주소를 적게 되어있는데요 이부분은 각각 알맞는 자신의 슬랙 webhook 주소를 적어주시면 됩니다. 

그 다음으로는 create monitor 를 해줘야하는데요. 이부분이 처음 접하는 저한테는 제일 어려웠던 부분인 것 같아요. 바로 ELK 쿼리를 json 형태로 만들어야 하는데요. 이부부은 저도 정확하게 잘 몰라서 검색을 하여 일단 기본설저응로 해두었어요. 

우선적으로 index 를 선택하고 쿼리를 설정해서 run 해보면 수행 결과도 볼수있으니 차근차근 검색해서 적용해보면 됩니다.  

![&#xC54C;&#xB78F;&#xC124;&#xC815;2](https://curtiskim.github.io/img/elk/103.png)

```text
{
    "size": 0,
    "query": {
        "bool": {
            "filter": [
                {
                    "range": {
                        "@timestamp": {
                            "from": "{{period_end}}||-10m",
                            "to": "{{period_end}}",
                            "include_lower": true,
                            "include_upper": true,
                            "format": "epoch_millis",
                            "boost": 1
                        }
                    }
                },
                {
                    "match": {
                        "level": {
                            "query": "ERROR",
                            "operator": "OR",
                            "prefix_length": 0,
                            "max_expansions": 50,
                            "fuzzy_transpositions": true,
                            "lenient": false,
                            "zero_terms_query": "NONE",
                            "auto_generate_synonyms_phrase_query": true,
                            "boost": 1
                        }
                    }
                }
            ],
            "adjust_pure_negative": true,
            "boost": 1
        }
    }
}
```

이제 다왔습니다. 이제 해당 모니터링 쿼리를 만들때 제일 하단 부분에 이제 interval 을 해줄 수 있습니다.

![&#xC54C;&#xB78F;&#xC124;&#xC815;3](https://curtiskim.github.io/img/elk/104.png)

* 분마다 조회하는 설정\(기본\) 으로 설정 

이제 위와 같이 설정을 끝 맞친 이후에 이제 create 를 하게되면 트리거 설정화면이 나오게 되어있습니다. 여기서는 이제 메세지가 실제로 가는지도 테스트 해볼수있으며 메세지에 어떻 값을 설정할지에 대해서도 셋팅을 할 수 있습니다.

![&#xC54C;&#xB78F;&#xC124;&#xC815;4](https://curtiskim.github.io/img/elk/105.png)

* 트리거 설정 화면

![&#xC54C;&#xB78F;&#xC124;&#xC815;5](https://curtiskim.github.io/img/elk/106.png)

* 메세지를 테스트 해볼 수 있는 화

이제 어플리케이션을 실행하고 로그를 남겨보면   다음과 같은 알람이 옵니다.   \( 슬랙 화면 \) 

![&#xBA54;&#xC138;&#xC9C0;&#xACB0;&#xACFC;](https://curtiskim.github.io/img/elk/107.png)

* 샘플은 이정도에서 하고 나중 정책에 따라 로그의 패턴을 정리해야 할 것 같구요
* 패턴 정리가 끝나면 이제 알람 기능도  조금 더 손봐야할 것 같네요 \( 메세지 로그레벨 등 \) 

감사합니다.

