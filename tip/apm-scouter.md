---
description: Application Performance Management
---

# APM scouter

안녕하세요 평범한 개발자 김봉준입니다.

  
APM 이란 어플리케이션 성능 모니터링 이라고 할 수 있습니다.  
여러가지가 있는데요 naver에서 개발한 오픈소스인 pinpoint , lg cns 에서 개발한 오픈소스 scouter ,  
이미 너무 유명한 상용솔루션 제니퍼 , 또한 아모레 회사에서 사용하고있는 인터맥스 , 엘라스틱 APM 등 이미 너무 유명한 apm 도구들이 있습니다.  
또한 aws 를 사용한다면 클라우드 와치를 사용할수 있겠지요  
위 와 같은 여러가지중 저는 스카우터 오픈소스 간략 사용법을 소개 하도록 하겠습니다.

우선적으로 스카우터 설치는 매우 간단 합니다.  [스카우터공식깃헙주소](https://github.com/scouter-project/scouter) 에 가면 설치 가이드 및 간략한 설정 방법이 나옵니다. 참고 부탁드립니다. 

## APM 스카우터 설치 및 활

* 우선적으로 APM 개념에 대해 간략하게 그림을 통해 배워보도록 하죠.

![&#xC2A4;&#xCE74;&#xC6B0;&#xD130; &#xAD6C;&#xC131;&#xB3C4;](https://curtiskim.github.io/img/scouter/100.png)

구성도는 위와  같습니다. 간략하게 설명을 드리자면 

* Client :  콜랙터에서 수집한것을 비쥬얼라이징해서 볼수있는 도구  \( 이클립스 기반으로 되어있는 오픈 소스 \) 
* Collector : 수집서버
* agent.java : 어플리케이션 모니터링 agent 
* agent.host : os 모니터링 agent 

대략적으로 위와같이만 이해하시면되요.   
과연 저 스카우터를 통해서 어떤정보를 볼수있는지 간략하게 샘플을 통해서 보도록 하겠습니다.  
우선적으로 제 로컬 mac os 장비에 Collector 서버를 구성하였습니다. 

### 설정

* 설정을 다음과 같아요

```text
# Agent Control and Service Port(Default : TCP 6100)
net_tcp_listen_port=6100
 
# UDP Receive Port(Default : 6100)
net_udp_listen_port=6100
 
# DB directory(Default : ./database)
db_dir=./database
 
# Log directory(Default : ./logs)
log_dir=./logs
 
# web ui
net_http_server_enabled=true
net_http_api_enabled=true
net_http_port=6180
```

위와 같이 간략한 설정을 합니다.

저는 여기서 디폴트 설정 이외의 web ui 설정을 하였는데요.  이부분은  스카우터에서  서드파티 ui 라이브러리로 paper 를 제공하여  web 에서 확인할 수 있는 UI를 제공할 예정입니다.   \( 제일 하단 대시보드 그림을 참조 하시면 될 것 같습니다. \) 

이제 수집서버 설정이 끝났으면 이제 수집서버를 기동해줍니다. \( startup.sh \)

![&#xAD6C;&#xB3D9;&#xD654;&#xBA74;](https://curtiskim.github.io/img/scouter/101.png)

위와같이 이렇게 수집서버를 기동한후에 이제 agent.host 를  이용하여 os 모니터링을 하도록하겠습니다. \( 방법은 매우 간단해요 \) 

공식 홈페이지 다운로드 받은 agent.host 를 수행시켜주기만 하면 됩니다.   \( host.sh\) 

```text
### scouter host configruation sample
net_collector_ip=127.0.0.1
net_collector_udp_port=6100
net_collector_tcp_port=6100
```

![&#xD638;&#xC2A4;&#xD2B8; &#xC218;&#xD589;&#xD654;&#xBA74;](https://curtiskim.github.io/img/scouter/102.png)

엄청 쉽죠?  이제 그럼 어플리케이션을 모니터링 해볼 차례인데요. 마찬가지로 매우 쉽습니다.   \( 간단해요 \)  

우선적으로 저는 통합상품인 샘플의 관리자 화면과 ,  api 서버를 모니터링 할 예정입니다.   
우선 콜랙터서버를 로컬에 구동하였으니 관리자 화면과 API 서버도 로컬에서 구동하도록 할께요 

참고로 샘플 프로젝트는 spring boot 기반으로 되어있는 어플리케이션입니다. 그럼 관리자 와 api 를 구동하기전 agent 설정을 봐보도록 하겠습니다. 

그러면 다운로드 받은 jar 파일의 설정파일인  agent.java  conf  를 수집서버를 바라보도록 설정을  해야합니다. 

```text
obj_name=local-sample-api
net_collector_ip=127.0.0.1
net_collector_udp_port=6100
net_collector_tcp_port=6100
profile_http_header_enabled=true
profile_http_parameter_enabled=true
profile_http_querystring_enabled=true
profile_spring_controller_method_parameter_enabled=true
trace_http_client_ip_header_key=X-Forwarded-For
```

* 위에서 보면 obj\_name 이 이제 콜랙터에서 확인할수있는 어플리케이션 이름이 될것이구요   
* IP 주소와 UDP ,TCP 포트를 지정해줍니다 
* 여기서 해더정보랑 ip\_header\_key 까지 다 지정할수있 도록 설정을 활성시켜줍니다.
  * 여기서 좀더 다양한 설정을 할수있는데요  자세한 내용은 공식 홈페이지를 확인해주세요 .
*  api 쪽과 동일하게 admin 도 설정파일을 만들어 주는데 이때 이름 부분은 local-admin 으로 설정해 줍니다  \( 이부분은 생략 \) 

  이렇게 되면 이제 어프리케이션을 구동할때 옵션을 줘야합니다.   \( 혹은 톰캣을 사용하고있다면  server.sh 에 자바옵션을 주면되요 \)  

  * \(여기서 java 이외의 다른 어플리케이션이라면 물론 가능합니다. 이것도 공식 홈페이지에 자세하게 나와있어요 \) 

아래와같은 옵션을 WAS 구동시 줘야합니다.

```text
-javaagent:/Users/amore/scouter/scouter/agent.java/scouter.agent.jar
-Dscouter.config=/Users/amore/scouter/scouter/agent.java/conf/scouter-admin.conf
```

* 깃헙에서 다운받은 agent.javr jar 파일 경로와 , 설정정보가 담긴 conf 파일을 tomcat 혹은 구동 jar 실행 옵션으로 JVM\_OPTION 을 지정해줘야 합니다. 

자 이제 끝이 났어요!! 어플리케이션을 구동하면 이제 수집서버에 정상적으로 수집이 될 것입니다. 

* 공식홈페이지 정보에 따르면 모니터링 대상은 다양합니다 . 아래를 참조해주세요. 

{% hint style="info" %}
* Monitoring targets \(from scouter agent\)
  * Java Agent : Web application \(on Tomcat, JBoss, Resin ...\), Standalone java application
  * Host Agent : Linux, Windows, Unix
* Monitoring targets \(from Telegraf support\) Since @2.0.0
  * Redis, nginX, apache httpd, haproxy, Kafka, MySQL, MongoDB, RabbitMQ, ElasticSearch, Kube, Mesos ...
* Monitoring targets \(from Zipkin-Scouter storage\) Since @2.5.0
  * Any zipkin instrumentations\(C\#, Go, Python, Javascript, PHP...\) can be shown in a XLog\(Scatter\) chart.
  * see the [zipkin-scouter-storage](https://github.com/scouter-project/zipkin-scouter) documentation.
  * see the [zipkin instrumentations.](https://zipkin.io/pages/extensions_choices.html)
{% endhint %}



자그럼 이제 수집한 데이터를 눈으로 확인할수있는 대시보드를 보도록 하겠습니다. 브라우저에서 그럼 이제 다음과 같이 대시보드를 확인해볼수 있습니다. \( 제 PC OS ,  API ,  ADMIN  \)

### 페이퍼 대시보

* 여기서 확인할수있는 정보는 다양합니다. 아래 참조로 스크린샷을 넣어뒀으니 참고용으로 사용해주시면되요.

![&#xC5F0;&#xACB0;&#xB418;&#xC5B4;&#xC788;&#xB294; &#xC11C;&#xBC84; &#xBAA9;&#xB85D;](https://curtiskim.github.io/img/scouter/103.png)

![XLOG &#xC5D0;&#xC11C; &#xD074;&#xB9AD;&#xD558;&#xC5EC; &#xBCFC;&#xC218;&#xC788;&#xB294; &#xC0C1;&#xC138; &#xC815;&#xBCF4;&#xB4E4;](https://curtiskim.github.io/img/scouter/104.png)

![&#xC2DC;&#xAC04; , &#xC720;&#xB2C8;&#xD06C; &#xBC29;&#xBB38;&#xC790; , &#xC11C;&#xBE44;&#xC2A4; &#xCE74;&#xC6B4;&#xD2B8; , &#xBA54;&#xBAA8;&#xB9AC; &#xC815;&#xBCF4; , CPU &#xC815;&#xBCF4;](https://curtiskim.github.io/img/scouter/105.png)

또한 GC 관련, process CPU, TPS, API TIME, SQL TIME, HEAP MEMORY   
이외의 추가적인 정보를 더 확인할수 있어요 

참고 페이퍼 사용때 

* 다음과 같은 모니터링 대시보드를 브라우저에서 확인하실 수 있습니다.
  * UI 페이퍼를 조작하는 방법은 공식 홈페이지에서 확인해보세요
* 위 대시보드는 다양하게 편집이 가능하며 조작이 가능합니다  , \( 드래그 앤 드랍 \) 
* 원하시는것을 볼수있게 레이아웃 설정도 가능합니다. 
* 원하시는 서버에대한 정보만 확인도 가능합니다. 
* 기간을 지정하여 수집된 자료를 과거도 볼수있습니다.
  * 제 기억으로는 해당 수집서버의 80% 디스크가 차게되면 과거 기록을 먼저 지우게 됩니다.
  * 디폴트 수집 기간이 정해져있습니다. \( 이부분도 하루동안만 수집 할수있으며 다양하게 설정가능\) 
  * 수집된것을 그라파나와 같은 것과 연동하여 30일치 요약본을 저장하여 또다른 대시보드로 꾸밀수도 있습니다. 
* APM 을 사용하는데 장애 알람이 오지 않는다면 약간 허무할듯하여 스카우터 서드파트 라이브러리 ?혹은 플러그인을 활용하여  슬랙과 연동하여 장애 알람을 어떻게 받는지 다음 시간에 알아보도록 하겠습니다. 
* 또한 OS CPU나 MEMORY 같은 정보는 AWS 클라우드와치를 다양한 정보를 대시보드를 만들어서 볼수있습니다.
  * aws console 에 접속해서 해당 부분을 확인하는 방법도 좋을 것 같네요 \( read 권한을 신청해서 비교해서 봐도 좋을것 같습니다. \)

## 알람 설정

위 부분에서 경고 알람을 설정을 하지 않고 넘어갔는데 이번에는 경고에 관련된 부분을 알아보자.

스카우터는 여러가지를 알랏 기능을 지원하는데,  샘플 예제는 슬랙 채널에 알람을 보내도록 구성을 하겠습니다. 

우선적으로 슬랙 설정을 하려고하면  jar 파일을 다운로드 받아줘야합니다.  [슬랙플러그인](https://github.com/scouter-contrib/scouter-plugin-server-alert-slack) 에서 해당 release 된 jar 파일 전부 받아줍니다.   \( 6개 \) 

* commons-codec
* commons-logging
* gson
* httpclient
* httpcore
* scouter-plougin-server-alert-slack 

위 파일을 다운로드 받은 이후에는 기존에 설치해두었던 scouter collector 서버의 lib 폴더 밑으로 다 옮겨줍니다.   
그리고 스카우터 수집서버를 재기동해야하는데요.   
이떄 재기동하기전에 슬랙 설정을 conf 파일에 적어줘야합니다.   
설정 파일을 적어주기전에 먼저 알랏기능으로 어떤 알랏을 보낼수 있는지 확인해보면 다음과 같습니

* CPU of Agent \(warning / fatal\) \#cpu 관련 알람
* Memory of Agent \(warning / fatal\) \# 메모리 관련 알람
* Disk of Agent \(warning / fatal\) \# 디스크 관련 알람
* connected new Agent \# 연결된 새 에이전트
* disconnected Agent \# 연결이 끊긴 에이전트
* reconnect Agent \# 재연결된 에이전트

서버의 scouter.conf  설정할 수 있는 부분은 다음과 같습니다. 

* 여기서 위와같은 알람 뿐만 아니라 특정 리퀘스트 응답시간이 느리게 되었을때도 알람을 받아볼 수 있습니다 . 

서버의 scouter.conf  설정할 수 있는 부분은 다음과 같습니다. 

* _ext\_plugin\_slack\_send\_alert_ : can send slack message or can'not \(true / false\) - default : false \# true 로 줘서 알람 설정을해줘야함.
* _ext\_plugin\_slack\_debug_ : can log message or can't - default false \# true 를 줘서 메세지를 기록하는것을 활성화 해야함
* _ext\_plugin\_slack\_level_ : log level \(0 : INFO, 1 : WARN, 2 : ERROR, 3 : FATAL\) - default 0 \# 로그레벨 - 2 이상으로 해도 무방할듯
* _ext\_plugin\_slack\_webhook\_url_ : Slack WebHook URL \# 슬랙 주소
* _ext\_plugin\_slack\_channel_ : \#Channel or @user\_id \#슬랙 채널 명
* _ext\_plugin\_slack\_botName_ : bot name \# 봇 이름을 지정할수있음
* _ext\_plugin\_slack\_icon\_emoji_ : Slack emoticon \(if it is existed slack emotion, it will use slack emotion \) \# 슬랙 이모지 지정
* _ext\_plugin\_slack\_icon\_url_ : icon image URL \# 아이콘도 커스텀할 수 있음
* _ext\_plugin\_elapsed\_time\_threshold_ : 응답시간의 임계치 \(ms\) -기본 값은 0으로, 0일때 응답시간의 임계치 초과 여부를 확인하지 않는다.
* _ext\_plugin\_gc\_time\_threshold_ : GC Time의 임계치 \(ms\) - 기본 값은 0으로, 0일때 GC Time의 임계치 초과 여부를 확인하지 않는다.
* _ext\_plugin\_thread\_count\_threshold_ : Thread Count의 임계치 - 기본 값은 0으로, 0일때 Thread Count의 임계치 초과 여부를 확인하지 않는다.
* _ext\_plugin\_slack\_xlog\_enabled_ : xlog maasege send \(true / false\) - default : false

그래서 테스트용도로 샘플 설정을 해본다면 다음과 같습니다.

```text
( 생략 )
...
# External Interface (Slack)
ext_plugin_slack_send_alert=true
ext_plugin_slack_debug=true
ext_plugin_slack_level=0
ext_plugin_slack_webhook_url=https://hooks.slack.com
#ext_plugin_slack_channel=
ext_plugin_slack_botName=scouter
ext_plugin_slack_icon_emoji=:computer:
ext_plugin_slack_icon_url=https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQnQFLCYQ-rg_iJFXBzazZjUqMXTHPmTQ-AVU_JymsxleUHI1Oe
ext_plugin_slack_xlog_enabled=true
ext_plugin_elapsed_time_threshold=3000  # 응답속도 3초 이상을 임계치로 지정한다.
ext_plugin_gc_time_threshold=5000 # GC 임계치 타임시간을 지정한다.
ext_plugin_thread_count_threshold=300  # 쓰레드 카운트 임계치를 지정한다.
```

이제 위와 같이 한후에 콜랙터 서버를 재기동합니다.

여기서 저는 메세지가 어떤식으로 오는지 확인 하기 위하여 샘플 API 를 1초에 동시사용자 350 명씩 3번 루프를 돌면서 보내는 설정으로  \( jmeter  이용 \)  샘플  api 호출을 해보도록 하겠습니다.

여기서 제 로컬에서 api 서버 구동을할때는 DB 케넥션 숫자도 10개이며 동시에 많은 사용자가 호출을 하기때문에 지연이 발생할것을 예상하고 지연이 발생했을때 슬랙 알람이 올것을 예상하고 테스트를 하였습니다.

그러면 이제 정상적으로 알람이 오는지 확인을 해보죠. 

우선적으로 위 설정을 하고 콜랙터 서버를 구동하게되면,  슬랙 알람방에 이제 agent 가 연결되었다는 알람이 오게 되어 있습니다. 

* 해당부분은 info 레벨의 로그로 남겨져서 오게 되어있습니다

![&#xC2AC;&#xB799; &#xC54C;&#xB78F; &#xB0B4;&#xC6A9;](https://curtiskim.github.io/img/scouter/106.png)

위와 같이  슬랙 채널에 알람이 오는것을 확인해볼 수 있습니다.

이제 그럼 워닝 알람도 정상적으로 오는지 확인해보기 위해서 jmenter 로 해당 api를 콜하도록하겠습니다. \( 1초  ,350 유저 ,3번반복 \) 

![&#xBC18;&#xBCF5; &#xC54C;&#xB78C; ](https://curtiskim.github.io/img/scouter/107.png)

그럼 이제 이렇게 응답 시간이 3초를 넘어가는 응답에 대한 알람만 오게되있어요.. \( 약 40여개 이상 의 알람 \) 

이렇게 알람설정을하여서 알람이 많이 오도록 하게 할수있습니다. 

### 참고 테스트 했을때 모니터링 대시보드 상황 참고

![XLOG](https://curtiskim.github.io/img/scouter/108.png)

![&#xB370;&#xC2DC;&#xBCF4;&#xB4DC; &#xBAA8;&#xC2B5;](https://curtiskim.github.io/img/scouter/109.png)

그럼 이제 간략한 알람 설정을 끝냈습니다.   
알람은 다양하게 설정할 수 있으며 이메일로도 받아볼수 있습니다.  
여기서 알람의 기준을 정할때 팀내 기준이 있다면 팀내 기준에 따라서 알람을 설정하는게 제일 좋을 것 같네요

장애가 아닌경우에 알람이 오게 될 경우 계속 불필요한 알람만 쌓이게되면 나중에 중요한 알람이 왔을경우 모르고 지나갈수 있는 여지가 생기니 알람 셋팅은 팀원들과 상의를 충분히 하고 설정하길 권장드립니다. 

위와 같은 알람은 사실 AWS CW 에서도 가능할테니 인프라팀의 지원을 받아서 프로젝트의 알람 설정을 해두는것도 좋은 방법 같아요. 



감사합니

