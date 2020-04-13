---
description: Spring Cloud 간단하게 따라해보기.
---

# Spring Cloud

안녕하세요 평범한 개발자 김봉준입니다.

{% hint style="info" %}
spring one seoul 다녀오고 이것저것 컨퍼런스에서 주서들은 내용들 중 spring cloud 관련된 부분

2019년 간략하게 정리했던 포스팅 개인공간으로 이동한 내용입니다.

본 내용은 spring one seoul 에서 발표한 세션과는 관련없는 내용인데요.

해당 컨퍼런스에 참여하고 나서 spring cloud 를 혼자 정리해본 내용입니다. 
{% endhint %}

#### \# spring api gateway <a id="SpringCloud-&#xAE30;&#xCD08;-#springapigateway"></a>

\#\# 해당 포스팅은.. 1일정도 spring io 와 각종 블로그들을 보며 확인한 내용들이라.. 다소 미흡한 점이 많을 수 있습니다.  
준비기간이 그리 길지 않게 시작하여 다소 두서가 없을수 있으며,  정확인 내용은 아닐 수 있으나,  
스프링 클라우드에 대해 맛보기 소개 정도로 생각해주시면 좋을 것 같습니다.

일단 저는 잘 모릅니다. 그래서 공식  홈페이지에 나와있는 프로젝트를 따라 만들어보면서 이해를 해보자. 라고 시작하게 되었습니다.

gateway 가 필요한 이유는 구글에서 검색해보면 여러가지가 나오는데  api gateway를 도입하게되면   
msa 환경에서의 반복적으로 발생하는 인증, 모니터링등을 중앙 단일화 를 할 수 있다고 한다!!!  라고하네요.

음 ...  
관련되어 대충 생각해보면  인증, 인가 등등이런것들을 gateway 가 하고 마이크로 서비스들은 딱 비즈니스 로직에만 집중하게 하고 격리할수 있는 것 같습니다.

또한  aws gateway 랑 zuul 이랑 장단 비교한 내용이 있는데 자세한 내용은 아래 블로그에 잘 나와있다. \( 한번 읽어보면 좋을 것 같아요 \)  [참고블로그](https://jsonobject.tistory.com/464)  
블로그에서 보면  api gateway를 넷플릭스 zuul 로 생성하는 방법이 자세하게 나오는데. 한번쯤 따라 해볼만 한 것 같습니다. 다들 따라해보시죠~ +\_+  
   
하지만 앞으로 spring cloud 에서 더이상 넷플릭스 zuul 을 지원하지 않는다고 라고 어디서 들은것같기도하고 블로그에서 본것 같기도 하여 필자는  둘다 잘모르니 최신꺼 해보자 라는 개인적인 생각도있고,  그래서 아래 대충 정리한  내용은 spring cloud gateway 로 샘플을 만들어 보았습니다.

우선 그래서 각 샘플의 특징을 간략하게나 확인해 보겠습니다.

####  Eureka \( [참고](https://cloud.spring.io/spring-cloud-netflix/reference/html/) \)     \* IP 로 등록 처리를 하는게 아니라 서비스 이름을 가지고 등록하고 중앙에서 관리하게 해줍니다.  \* 새로운 서비스가 시작될때 중앙 의 서비스 레지스트리에 등록 되는 형태  \* 유레카 서버는 service id 랑 url 을 가지고 있습니다.  \* 각각의 MSA 서버가 시작될때 유레카 레지스트리에 등록하게 되어 있습니다. \( 유레카 클라이언트를 통해 \)   \* Discovery Client 는 서버로부터\(유레카\) 레지스트리 정보를 읽어와 로컬 캐시에 저장 합니다. \( Default 30초 \)   <a id="SpringCloud-&#xAE30;&#xCD08;-###Eureka(&#xCC38;&#xACE0;)*IP&#xB85C;&#xB4F1;&#xB85D;&#xCC98;&#xB9AC;&#xB97C;&#xD558;&#xB294;&#xAC8C;&#xC544;&#xB2C8;&#xB77C;&#xC11C;&#xBE44;&#xC2A4;&#xC774;&#xB984;&#xC744;&#xAC00;&#xC9C0;&#xACE0;&#xB4F1;&#xB85D;&#xD558;&#xACE0;&#xC911;&#xC559;&#xC5D0;&#xC11C;&#xAD00;&#xB9AC;&#xD558;&#xAC8C;&#xD574;&#xC900;&#xB2E4;.*&#xC0C8;&#xB85C;&#xC6B4;&#xC11C;&#xBE44;&#xC2A4;&#xAC00;&#xC2DC;&#xC791;&#xB420;&#xB584;&#xC911;&#xC559;&#xC758;&#xC11C;&#xBE44;&#xC2A4;&#xB808;&#xC9C0;&#xC2A4;&#xD2B8;&#xB9AC;&#xC5D0;&#xB4F1;&#xB85D;&#xB418;&#xB294;&#xD615;&#xD0DC;*&#xC720;&#xB808;&#xCE74;&#xC11C;&#xBC84;&#xB294;serviceid&#xB791;url&#xC744;&#xAC00;&#xC9C0;&#xACE0;&#xC788;&#xB2E4;*&#xAC01;&#xAC01;&#xC758;MSA&#xC11C;&#xBC84;&#xAC00;&#xC2DC;&#xC791;&#xB420;&#xB54C;&#xC720;&#xB808;&#xCE74;&#xB808;&#xC9C0;&#xC2A4;&#xD2B8;&#xB9AC;&#xC5D0;&#xB4F1;&#xB85D;&#xD558;&#xAC8C;&#xB418;&#xC5B4;&#xC788;&#xB2E4;.(&#xC720;&#xB808;&#xCE74;&#xD074;&#xB77C;&#xC774;&#xC5B8;&#xD2B8;&#xB97C;&#xD1B5;&#xD574;)*Discoveryclient&#xB294;&#xC11C;&#xBC84;&#xB85C;&#xBD80;&#xD130;(&#xC720;&#xB808;&#xCE74;)&#xB808;&#xC9C0;&#xC2A4;&#xD2B8;&#xB9AC;&#xC815;&#xBCF4;&#xB97C;&#xC77D;&#xC5B4;&#xC640;&#xB85C;&#xCEEC;&#xCE90;&#xC2DC;&#xC5D0;&#xC800;&#xC7A5;&#xD55C;&#xB2E4;(default30&#xCD08;)"></a>

#### \#\#\# Gateway \( [참고](https://cloud.spring.io/spring-cloud-gateway/reference/html/) \)  \* MSA 의 컴포넌트중 하나이며 , 모든 클라이언트 요청에 대한 End Point 통합하는 서버입니다.  \( proxy server 비슷..\) \* Route\(경로\) : 게이트웨이의 기본 빌딩 블록을 라우팅합니다. 이것은 ID, 목적지 URI, 조건부\(predicate\)의 집합 및 필터들의 집합으로 정의됩니다. 종합\(aggegate\)된 조건부가 참일 경우 경로를 매치합니다. \* Predicate\(조건부\) : 이것은 Java 8 의 Function Predicate 입니다. 입력 유형은 Spring Framework SeverWebExchange 입니다. 개발자가 헤더나 파라미터 등과 같은 HTTP 요청으로부터 어떤 것이든 매치할 수 있도록 해 줍니다. \* Filter\(필터\) : 특정한 factory 로 생성된 Spring Framework GatewayFilter 인스턴스입니다. 이곳에서 내려보내기\(downstream\) 요청을 보내기 전후에 변경을 가할 수 있습니다. <a id="SpringCloud-&#xAE30;&#xCD08;-###Gateway(&#xCC38;&#xACE0;)*MSA&#xC758;&#xCEF4;&#xD3EC;&#xB10C;&#xD2B8;&#xC911;&#xD558;&#xB098;&#xC774;&#xBA70;,&#xBAA8;&#xB4E0;&#xD074;&#xB77C;&#xC774;&#xC5B8;&#xD2B8;&#xC694;&#xCCAD;&#xC5D0;&#xB300;&#xD55C;endpoint&#xD1B5;&#xD569;&#xD558;&#xB294;&#xC11C;&#xBC84;&#xC774;&#xB2E4;.(proxyserver&#xBE44;&#xC2B7;..)*Route(&#xACBD;&#xB85C;):&#xAC8C;&#xC774;&#xD2B8;&#xC6E8;&#xC774;&#xC758;&#xAE30;&#xBCF8;&#xBE4C;&#xB529;&#xBE14;&#xB85D;&#xC744;&#xB77C;&#xC6B0;&#xD305;&#xD569;&#xB2C8;&#xB2E4;.&#xC774;&#xAC83;&#xC740;ID,&#xBAA9;&#xC801;&#xC9C0;URI,&#xC870;&#xAC74;&#xBD80;(predicate)&#xC758;&#xC9D1;&#xD569;&#xBC0F;&#xD544;&#xD130;&#xB4E4;&#xC758;&#xC9D1;&#xD569;&#xC73C;&#xB85C;&#xC815;&#xC758;&#xB429;&#xB2C8;&#xB2E4;.&#xC885;&#xD569;(aggegate)&#xB41C;&#xC870;&#xAC74;&#xBD80;&#xAC00;&#xCC38;&#xC77C;&#xACBD;&#xC6B0;&#xACBD;&#xB85C;&#xB97C;&#xB9E4;&#xCE58;&#xD569;&#xB2C8;&#xB2E4;.*Predicate(&#xC870;&#xAC74;&#xBD80;):&#xC774;&#xAC83;&#xC740;Java8&#xC758;FunctionPredicate&#xC785;&#xB2C8;&#xB2E4;.&#xC785;&#xB825;&#xC720;&#xD615;&#xC740;SpringFr"></a>

\* 유명한 그림 \(출처  :   [https://cloud.spring.io/spring-cloud-static/spring-cloud-gateway/2.1.0.RELEASE/single/spring-cloud-gateway.html](https://cloud.spring.io/spring-cloud-static/spring-cloud-gateway/2.1.0.RELEASE/single/spring-cloud-gateway.html) \) 

![105](https://curtiskim.github.io/img/spring-cloud/105.png)

여기서 Gateway 를  만들게 되면 좋은게  여러가지 기능을 합니다    \(  위에 첨부한 참고 url 가보게 되면 \( spring io \)  \)  
특정 시간 이후부터 api 요청의 endpoint 바꿀 수 도있으며  , 특정기간동안의 endpoint 또한 변경 할 수 있다 \( 점검 페이지 등을 만들수 있음.\)  
또한 , 파라미터 , 메소드 , 쿠키, 헤더정보, 호스트, ip .... 등에 따른 endpoint 를 변경할수있다.  꽤 좋은 것 같습니다.  
-&gt; 자세한건 공식 홈페이지를 확인해보시죠!!

다운스트림에 내려줄때는 proxy 등 처리할때 x-forwarded-\* 같은 민감 헤더정보를 제거할 수 있다고 합니다.

또한 게이트 웨이를 사용하면 , msa로 분산되어있는 서비스에대한 도메인을 하나로 합칠 수도 있다고 하네요.  
여러가지 필터를 이용해 볼수 있을것 같아요. \(통계 제공 ,  인증식별 ,  만족하지 않는 요청은 거부 , 동적 라우팅 ,  트레픽 관리 , \)   
gateway 는 httpservlet 의 구현체 인 것 같습니다.

#### \#\#\# Config \* 외부 구성을 위한 http, 리소스 기반 api   \* 속성 값 암호회 \(암호화 또는 비대칭 \) \* 다음을 사용하여 Spring Boot 애플리케이션에 쉽게 임베드 가능  @EnableConfigServer  <a id="SpringCloud-&#xAE30;&#xCD08;-###Config*&#xC678;&#xBD80;&#xAD6C;&#xC131;&#xC744;&#xC704;&#xD55C;http,&#xB9AC;&#xC18C;&#xC2A4;&#xAE30;&#xBC18;api*&#xC18D;&#xC131;&#xAC12;&#xC554;&#xD638;&#xD68C;(&#xC554;&#xD638;&#xD654;&#xB610;&#xB294;&#xBE44;&#xB300;&#xCE6D;)*&#xB2E4;&#xC74C;&#xC744;&#xC0AC;&#xC6A9;&#xD558;&#xC5EC;SpringBoot&#xC560;&#xD50C;&#xB9AC;&#xCF00;&#xC774;&#xC158;&#xC5D0;&#xC27D;&#xAC8C;&#xC784;&#xBCA0;&#xB4DC;&#xAC00;&#xB2A5;@EnableConfigServer"></a>

### \#\# 1.spring discovery 구성

우선적으로 유레카 서버를 만들어 보겠습니다. 

우선 의존성은 아래와 같이 추가합니다. 

```text
dependencies {
   implementation 'org.springframework.cloud:spring-cloud-starter-netflix-eureka-server'
 
   // jdk 11  support in eureka
   implementation 'com.sun.xml.bind:jaxb-core:2.3.0.1'
   implementation 'com.sun.xml.bind:jaxb-impl:2.3.0'
//    compile 'javax.xml.bind:jaxb-api:2.3.0'
   implementation group: 'javax.xml.bind', name: 'jaxb-api', version: '2.4.0-b180725.0427'
   testImplementation 'org.springframework.boot:spring-boot-starter-test'
}
```

\* 여기서 많은 예제들은 java 1.8 로 되어 있습니다.   
하지만 필자는 java 12를 사용하였는데  java 11부터 jaxb 가 remove 되었습니다.  
그래서 유레카 서버 의존성을 위와같이 jaxb 를 따로 추가해줘야 정상적으로 동작이 될 것 입니다. \( 유레카에서 내부적으로 jaxb를 사용하는데 jdk 11부터는 remove 가 되어서 시작할때 tomcat 관련 에러가 발생 \) 

### 1. application.yml 설정

```text
server:
  port: 8761
 
spring:
  application:
    name: ap-euraka-discovery
  profiles:
    active: local
 
eureka:
  client:
    fetchRegistry: false
 
---
# 로켈 셋
spring:
  profiles: local
```

\* application name을 지정해줍니다.

### 2.application.java

```text
@SpringBootApplication
@EnableEurekaServer
public class ApDiscoveryApplication {
 
 public static void main(String[] args) {
  SpringApplication.run(ApDiscoveryApplication.class, args);
 }
 
}
```

@EnableErekaServer 어노테이션으로 유레카 서비스를 활성화 합니다.  그리고 이제 해당 서버를 띄운 후에 localhost:8761 에 접근해보자. 그러면 이제 다음과 같은 화면이 나올게 됩니다.

![](https://curtiskim.github.io/img/spring-cloud/106.png)

위에서 보면 레지스트리에 들어간 서버는 한개로 확인 할 수 있습니다.  
초간단 유레카 서버는 완성 되었습니다.

이제 다음단계로 넘어가볼까요 

### \#\# 2.spring cloud config  구성

스프링 클라우드 컨피그 설정도 유레카와 비슷하게 프로젝트의 설정파일을 중앙에서 다 관리할수 있게 해주는데 목적이 있는 것 같아요  
우선 제가 만든 예제에서는 github 와 연동은 하지 않았는데.. \(찾아보면 예제가 간단하여 누구나 근방 할 수 있습니다.\)

그래도 대략적인 흐름을 알아보기 위해 로컬에만 셋팅을 하여 테스트를 진행해 보겠습니다.

###  2-1 의존성

```text
dependencies {
    implementation 'org.springframework.cloud:spring-cloud-starter-netflix-eureka-client'
    implementation 'org.springframework.cloud:spring-cloud-config-server'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
}
```

여기서도 유레카 클라이언트를 이용하여 서버가 스타트 될때 유레카한테 알려줍니다.

### 2-2 bootstrap.yml

```text
server:
  port: 9000
spring:
  profiles:
    active: native,local
  application:
    name: ap-config-service
eureka:
  client:
    registryFetchIntervalSeconds: 5
    enabled: true
    serviceUrl:
      defaultZone: ${EUREKA_URI:http://localhost:8761/eureka}
  instance:
    preferIpAddress: true

---
# 로컬 셋
spring:
  profiles: local
  cloud:
    config:
      server:
        native:
          #          search-locations: file://Users/amore/IdeaProjects/ap-config-server/src/main/resources/server-configs
          search-locations: classpath:/server-configs
```

2-1. application name 을 이용하여 유레카에 등록될 서비스 이름을 등록합니다.  
2-2 유레카 정보를 적어 줍니다.

```text
registryFetchIntervalSeconds = 기본 30초이지만 5초 단위로 재갱신 합니다.
```

[classpath:/server-configs](http://classpath/server-configs) 에는 각종 msa 설정파일을 가지고 있습니다. \(우리가 만들어야할 서비스들 다음과 같습니다.\) 

2-3. api gateway  \( ap-gateway-service-local.yml \)  
2-4. 회원 \( ap-member-service-local.yml \)   
2-5. 상품. \( ap-product-service-local.yml \)

그럼 각가의 설정 파일을 이제 만들어보겠습니다.

### 1. ap-gateway--service-local.yml

```text

spring:
  profiles: local
  cloud:
    gateway:
      routes:
      - id: ap-members-service
        uri: http://localhost:8081
        predicates:
        - Path=/members/**
      - id: ap-products-service
        uri: http://localhost:8080
        predicates:
          - Path=/products/**
 
eureka:
  client:
    serviceUrl:
      defaultZone: ${EUREKA_URI:http://localhost:8761/eureka}
  instance:
    preferIpAddress: true
```

게이트웨이 설정을 보게되면 이제 spring gateway 요청에 path 가 다음과 같다면  members/\*\*  localhost:8081 호스트로 보내게 됩니다..   
여기서 routes id 당 redicates 는 필수다. 꼭 적어줘야한다.  자세한 것들은   \( [https://cloud.spring.io/spring-cloud-gateway/reference/html/](https://cloud.spring.io/spring-cloud-gateway/reference/html/)\) 여기에 나와 있습니다.

### 2.ap-member-service-local,yml

```text
spring:
  profiles: local
  message: Member Service
 
eureka:
  client:
    serviceUrl:
      defaultZone: ${EUREKA_URI:http://localhost:8761/eureka}
  instance:
    preferIpAddress: true
```

\* 회원 MSA 에서 사용할 설정정보

### 3.ap-product-service-local.yml

```text
spring:
  profiles: local
  message: Product Service
 
eureka:
  client:
    serviceUrl:
      defaultZone: ${EUREKA_URI:http://localhost:8761/eureka}
  instance:
    preferIpAddress: true
```

\* 상품 MSA 에서 사용할 설정정보

{% hint style="info" %}
\*\*\* 기타 :  여기서  server:  하위에 git 주소를 적게되면 이제 git 에서 각 MSA 서비스 별로 설정파일 \(환경별\) 로 관리 할 수 있습니다.   \( 일단 크게 어려운게 아니니 넘어가보도록 하겠습니다.\)   
server:  
    git:  
        uri: "git 주소를 적어요~!!"
{% endhint %}

### 3.application.java

```text

@SpringBootApplication
@EnableConfigServer
@EnableDiscoveryClient
public class ApConfigServerApplication {
 
  public static void main(String[] args) {
    SpringApplication.run(ApConfigServerApplication.class, args);
  }
 
}
```

어노테이션을 이용하여 유레카 디스커버 클라이언트로 등록해주고 컨피그 서버를 활성화 합니다.

이제 spring cloud gateway 를 만들어 보기 전에 config 서버를 한번 띄워 봅니다.  \( 유레카에 잘 등록되는지도 확인할 겸 \)   
ap-confg-service 를 시작하게 되면 아래와 같이 ap-config-service 로 등록된걸 확인 할수 있습니다.

![](https://curtiskim.github.io/img/spring-cloud/107.png)

이제 이 포스팅의 목적인 gateway 를 만들어 보시죠.!!

### \#\# 3. spring gateway 구성

```text
implementation 'org.springframework.cloud:spring-cloud-starter-config'
implementation 'org.springframework.cloud:spring-cloud-starter-gateway'
implementation 'org.springframework.cloud:spring-cloud-starter-netflix-eureka-client'
testImplementation 'org.springframework.boot:spring-boot-starter-test'
```

1. bootstrap.yml

```text
server:
  port: 9100
spring:
  application:
    name: ap-gateway-service
 
  profiles:
    active: local
 
---
# 로컬
spring:
  profiles: local
  cloud:
    config:
      fail-fast: true
#      discovery:
#        service-id: ap-config-service
      uri: http://localhost:9000
```

\* 여기서 필자는 discovery: service-id : 로 ap-config-service 를 등록해줬는데 이상하게 여기서만 못찾았습니다.  그래서  강제적으로 [http://localhost:9000](http://localhost:9000/) 으로 config 서버를 하드코딩 했습니다. 그러니 돌아가긴 했는데 사실 네이밍으로  될 것 같은데... 나중에 다시 성공하면 위 내용을 바꿔 두도록 하겠습니다. 

3. application.java

```text
@SpringBootApplication
@EnableDiscoveryClient
public class ApGatewayApplication {
 
  public static void main(String[] args) {
    SpringApplication.run(ApGatewayApplication.class, args);
  }
 
}
```

마찬가지로 디스커버리클라이언트 어노테이션을 이용하여 서버 시작시 유레카에 알려줍니다.  
여기서 이제 우리는 설정파일을  위에 설정한  cloud config server 를 이용하기 때문에 

application name 을 이용하여 서버가켜질때  http://컨피그서버/어플리케이션명/환경  -&gt; [http://localhost:9000/ap-gateway](http://localhost:9000/ap-gateway-local)-service/local 을 호출하여 설정 정보를 가지고 오게 되어 있습니다.  
이제 그렇게되면 위에서 config 서버 만들어줄때 만들어둔 ap-gateway-service-local.yml 파일을 읽어서 설정 정보를 가지고 옵니다.

이제 게이트웨이를 시작 해봅니다.  마찬가지로 이번에도 유레카에 잘 등록 될 것 입니다.  \(스샷은 생략 \)   
간단한 gateway 설정도 이제 끝이 났습니다.

### \#\# 4. MSA Service Module 구성 <a id="SpringCloud-&#xAE30;&#xCD08;-##4.MSAServiceModule&#xAD6C;&#xC131;"></a>

이제 실제로 gateway 가 일을 잘하는지 확인 하기 위하여 상품 서비스와 , 회원 서비스를 만들어 주도록 합니다.  
상품 서비스 , 회원 서비스는 간단하기때문에  빠르게 넘어가보도록 하겠습니다.

### \* 회원 서비스 

### 1. 의존성

```text
implementation 'org.springframework.boot:spring-boot-starter-webflux'
implementation 'org.springframework.cloud:spring-cloud-starter-config'
implementation 'org.springframework.cloud:spring-cloud-starter-netflix-eureka-client'
testImplementation 'org.springframework.boot:spring-boot-starter-test'
testImplementation 'io.projectreactor:reactor-test'
```

### 4-1 applicationyml

```text
server:
  port: 8081
 
spring:
  profiles:
    active: local
  application:
    name: ap-member-service
 
management:
  endpoints:
    web:
      exposure:
        include: info ,refresh
 
---
# 로컬 셋
spring:
  profiles: local
  cloud:
    config:
      fail-fast: true
      discovery:
        service-id: ap-config-service
        enabled: true
```

마찬가지로 여기도  ap-config-service 에서 설정정보를 가지고 옵니다.  \( 엑추에이터 설정은 무시해주세요. 나중에 따로 잘 정리 해보도록 하겠습니다. management 부분 \) 

### 4-1 application.java

```text
@SpringBootApplication
@EnableDiscoveryClient
public class ApMemberApplication {
 
 public static void main(String[] args) {
  SpringApplication.run(ApMemberApplication.class, args);
 }
 
}
```

### 4-2 간단한 rest controller

```text
/**
 * @author bongjun.kim
 * @since 2019-09-06
 */
@RestController
@RequestMapping("members")
@Slf4j
public class MemberController {
 
  @Value("${server.port}")
  private int port;
 
  @Value("${spring.message}")
  private String message;
 
  @GetMapping
  public String getMembers(){
    log.debug("회원 서비스 입니다. port : {} , message : {}" ,port , message);
    return "회원 서비스 조회 , 이서버 포트는 >> "  + port + " By " + message ;
  }
 
  @GetMapping("{memberId}")
  public String getMember(@PathVariable("memberId") String memberId) {
    log.debug("회원 서비스 입니다. port : {} , message : {}" ,port , message);
    return "조회한 회원 정보는 "  + memberId;
  }
 
}
```

### 5 상품서비스

### 5-1 의존성

```text
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-webflux'
    implementation 'org.springframework.cloud:spring-cloud-starter-config'
    implementation 'org.springframework.cloud:spring-cloud-starter-netflix-eureka-client'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
    testImplementation 'io.projectreactor:reactor-test'
}
```

### 5-2 bootstrap.yml

```text

server:
  port: 8080
 
spring:
  application:
    name: ap-product-service
 
---
# 로컬 셋
spring:
  profiles: local
  cloud:
    config:
      fail-fast: true
      discovery:
        service-id: ap-config-service
        enabled: true
```

### 5-3 application.java

```text
@SpringBootApplication
@EnableDiscoveryClient
public class ApProductApplication {
 
  public static void main(String[] args) {
    SpringApplication.run(ApProductApplication.class, args);
  }
 
}
```

### 5-4 product rest controller

```text
/**
 * @author bongjun.kim
 * @since 2019-09-06
 */
@RestController
@RequestMapping("products")
@Slf4j
public class ProductController {
 
  @Value("${server.port}")
  private int port;
 
  @Value("${spring.message}")
  private String message;
 
  @GetMapping
  public String getProducts(){
    log.debug("상품 서비스 입니다. port : {} , message : {}" ,port , message);
    return "상품 서비스 조회 , 이서버 포트는 >> "  + port + " By " + message ;
  }
 
  @GetMapping("{productId}")
  public String getProducts(@PathVariable("productId") String productId){
    log.debug("상품 서비스 입니다. port : {} , message : {}" ,port , message);
    return "조회한 상품 정보는 "  + productId;
  }
 
}
```

### !!! 됬다!!!  이제 샘플 게이트웨이 테스트를 위한 준비는 끝났다. member service ,  product service 를 구동봅니다.  <a id="SpringCloud-&#xAE30;&#xCD08;-!!!&#xB42C;&#xB2E4;!!!&#xC774;&#xC81C;&#xC0D8;&#xD50C;&#xAC8C;&#xC774;&#xD2B8;&#xC6E8;&#xC774;&#xD14C;&#xC2A4;&#xD2B8;&#xB97C;&#xC704;&#xD55C;&#xC900;&#xBE44;&#xB294;&#xB05D;&#xB0AC;&#xB2E4;.memberservice,productservice&#xB97C;&#xAD6C;&#xB3D9;&#xD574;&#xBCF4;&#xC790;."></a>

이제 구동되어있는 5개의 서비스를 유레카에서 확인이 가능합니다. 

이제 gateway 서비스에  요청을 보내보면 각각으로 잘 전달하는걸 확인할 수 있습니다. 

![](https://curtiskim.github.io/img/spring-cloud/108.png)

```text
curl -XGET localhost:9100/members
회원 서비스 조회 , 이서버 포트는 >> 8081 By Member Service%    
```

```text
cloud curl -XGET localhost:9100/products
상품 서비스 조회 , 이서버 포트는 >> 8080 By Product Service%  
```

예상한데로 각각의 MSA 서버에  잘 전달하는 것 같습니다.  아주 간단하게 게이트 웨이를 구성하기 튜토리얼을 해봤습니다. 이제 남은 부분은 filter 인증 관련된 것도 하면 좋을것 같은데요. 이부분은 나중에 정리해서 올려두도록 하겠습니다.   


스프링 클라우드는 이제는 한글자료가 생각보다 꽤 많이 나왔으며 여러곳에서 적용중에 있는것 같아요. 많은 블로그에서 소개 되어있으며, 여러 회사 기술블로그에도 자주 등장하는 내용이니 위에 내용을 한번쯤 따라해보는것도 좋은 내용일것 같습니다. 

감사합니다.

