---
description: spring cloud config
---

# Spring Cloud Config

안녕하세요 평범한 개발자 김봉준입니다.

{% hint style="info" %}
본 포스팅은 spring cloud config 관련 내용입니다.
{% endhint %}

우선적으로 spring cloud config 를 어떻게 활용해볼까를 고민하다가 application.yml 의 상속?? 우선순위?에 대한 내용으로 정리를 해보도록 하겠습니다.   
우선적으로 그리하여 config 서버 관련 설정을 하면서  구글링을 해보고 직접 코딩을 해본 결과 

저는 {어플리케이션명}.yml ,  {어플리케이션명}-{환경}.yml  로 이루어져서 각각의 어플리케이션들 끼리는 환경을 공유하고 상속을 받을 수 있다고 생각하고 그렇게 이해를 하고 넘어갔습니다.  \( 무언가 결핍되어있는 내용으로 제가 이해를 하고 있던것 같아요.\)

글로벌하게 application.yml 파일을 두게되면 각각의 서비스들에서 공통으로 사용할 수 있는 것을 모아둘 수 있다고 하였습니다. - [공식문서](https://cloud.spring.io/spring-cloud-config/reference/html/)  


#### 자 이제 글만 보면 이해가 안갈 수 있으니 코딩을 해본 내용을 아래 정리해 보겠습니다.  config server  다음과 같은 어플리케이션 - 환경 . yml 설정 파일을 만들어 둔다고 가정하겠습니다.   \( 각 설정 정보는    [Spring Cloud - 기초](https://curtiskim.gitbook.io/notes/spring/spring-cloud) 에 나와있기 때문에 생략 하도록 하겠습니다. \)  <a id="SpringCloud-Config-&#xC790;&#xC774;&#xC81C;&#xAE00;&#xB9CC;&#xBCF4;&#xBA74;&#xC774;&#xD574;&#xAC00;&#xC548;&#xAC08;&#xC218;&#xC788;&#xC73C;&#xB2C8;&#xCF54;&#xB529;&#xC744;&#xD574;&#xBCF8;&#xB0B4;&#xC6A9;&#xC744;&#xC544;&#xB798;&#xC815;&#xB9AC;&#xD574;&#xBCF4;&#xACA0;&#xC2B5;&#xB2C8;&#xB2E4;.configserver&#xB2E4;&#xC74C;&#xACFC;&#xAC19;&#xC740;&#xC5B4;&#xD50C;&#xB9AC;&#xCF00;&#xC774;&#xC158;-&#xD658;&#xACBD;.yml&#xC124;&#xC815;&#xD30C;&#xC77C;&#xC744;&#xB9CC;&#xB4E4;&#xC5B4;&#xB454;&#xB2E4;&#xACE0;&#xAC00;&#xC815;&#xD558;&#xACA0;&#xC2B5;&#xB2C8;&#xB2E4;.(&#xAC01;&#xC124;&#xC815;&#xC815;&#xBCF4;&#xB294;&#xC5D0;&#xB098;&#xC640;&#xC788;&#xAE30;&#xB54C;&#xBB38;&#xC5D0;&#xC0DD;&#xB7B5;&#xD558;&#xB3C4;&#xB85D;&#xD558;&#xACA0;&#xC2B5;&#xB2C8;&#xB2E4;.)"></a>

![](https://curtiskim.github.io/img/spring-cloud/109.png)

여기서 위 설정대로 config 서버를 띄운 후에  회원 서비스를 띄운다고 가정한다면 회원서비스는 다음과 같은 config 정보를 참조하게 될 것 입니다.  
[http://localhost:8888/ap-member-service/local](http://localhost:8888/ap-member-service/local)  \( 도메인 / 어플리케이션명 / 환경 \) 

![](https://curtiskim.github.io/img/spring-cloud/110.png)

\( 예상대로 3개를 보게 되있습니다. 여기서 겹치는 부분이 있으면 우선순위에 의해서 우선순위가 높은게 적용된다고 보시면 될 것 같습니다. . application.yml  &gt; ap-member-sergvice.yml  &gt; ap-member-service-local.yml \)

여기서 위 스크린샷 내용만 보더라도 알수 있습니다. 

하지만  그래도 혹시모르니 한번더 확인 해보겠습니다.  \( 상품 서비스도 띄운다고 가정 \)   
이렇게 된다면  만약 상품서비스 ap-product-service 가 존재한다면, 이제  공통 부분은  application.yml 을 참조하게 될 것으로 예상되는데 실제로 그렇게 되는지 테스트 해보겠습니다.   
\( 상품 설정은  [Spring Cloud - 기초](https://curtiskim.gitbook.io/notes/spring/spring-cloud) 에 나와있기 때문에 생략 하도록 하겠습니다. \) 

[http:/localhost:8888/ap-product-service/local](http://http/localhost:8888/ap-product-service/local)  로 가보게 된다면 다음과 같이 나오게 될 것 입니다.

![](https://curtiskim.github.io/img/spring-cloud/111.png)



실제로 config-server 에  application.yml을 잘 가져옵니다. 

이제 요약을 해본다면   
**1.  config 서버를 활용하여  msa 서비스가 많이 늘어나도 공통 부분은 application.yml 에 두고**    
**2.  각각의서비스별 공통 부분은 ap-member-service.yml 의 파일에 두고**   
**3.  각 환경별 \(dev, stg, prd\)  ap-member-service-local.yml 의 파일에 두고 사용하면 좋을 것 같습니다.** 

spring - cloud - config 는 이렇게 

**분산되어있는 시스템에서 구성해 해야할 환경파일을 중앙에서 모두 관리하게 해주며 , 공통적인 부분은 한곳에 몰 수 있어서 좋은 것 같습니다.**   
\( 모든 애플리케이션이 함께 쓸수 있다는게 장점인 것 같습니다. \) 

다들 이미 알고 계셨겠지만 한번 정리 해봤습니다. 감사합니다.

