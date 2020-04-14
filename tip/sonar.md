---
description: sonarqube 소스 정적 분석도구
---

# 소나큐브 - 소스코드 정적분석도구

안녕하세요 평범한 개발자 김봉준 입니다.

{% hint style="info" %}
코드 정적분석도구로는 여러개가 있습니다. 

[PMD](https://pmd.github.io/) , [FindBugs](http://findbugs.sourceforge.net/) , [CheckStyle](http://checkstyle.sourceforge.net/) , [SonarQube](https://www.sonarqube.org/) 등등

각각 특색이 있는데요. 각 연결해둔 공식사이트에서 확인이 가능합니다.
{% endhint %}

### 들어가며

우선 이번 포스팅에서는 회사 팀내에서 사용하고있는  sonarqube 에 대해서 아주 간략하게 정리 해볼려고 합니다. 

보통 소나큐브를 사용할때에 jenkins + gitlab + webhook + sonarQube  조합으로 소스 코드 분석을 하고 있는 것 으로 알고 있습니다

### 특징 <a id="id-&#xC18C;&#xC2A4;&#xCF54;&#xB4DC;&#xC815;&#xC801;&#xBD84;&#xC11D;&#xB3C4;&#xAD6C;-&#xD2B9;&#xC9D5;"></a>

소나 큐브는 다음과 같은 특징이 있습니다.

1. 신뢰성 : 까다로운 버그를 잡아서 정의되지 않은 동작이 최종사용자에게 영향을 미치지 않게 한다
2. 보안 : 앱을 손상시키는 취약점을 해결
3. 기술 부채 : 코드베이스를 깨끗하게 유지 관리하게 해준다.

저도 이회사에 왔을때 처음 한 프로젝트를 진행 할때 해당 룰을 적용시켜서 MR 베이스 리뷰를 소나를 이용하려고 했습니다. 

하지만 회사네 정보보안이 관리하는 젠킨스와, 제가 사용하고있던 저희 깃랩, 개발팀에서 운영하고 있던  소나 서버 간의 방화벽 문제로 그냥 로컬에서 MR 날리기 전에 소나린트를 수동으로 확인하여 커밋 했습니다. \( 사실 로컬 lint를 많이 이용하였습니다. lint에 관한 부분은 아래 설명에 달아 두도록 하겠습니다.  **개인적인 생각으로는 lint 만 팀내에서 잘 사용한다면 충분하다고 생각합니다. \)**

### 기본 셋팅 <a id="id-&#xC18C;&#xC2A4;&#xCF54;&#xB4DC;&#xC815;&#xC801;&#xBD84;&#xC11D;&#xB3C4;&#xAD6C;-&#xAE30;&#xBCF8;&#xC14B;&#xD305;"></a>

뭐 일단 우리 팀에서 소나 큐브를 가지고있으니 팀내 소나큐브 서버에 샘플 소스를 올려서 결과를 확인 해보도록 하겠습니다.  테스트 용도로 사용할 샘플 프로젝트는 빌드도구로 gradle 을 사용하고 있습니다. 

gradle sonar 를 사용하기 위해서 몇가지 스크립트를 작성하여야 하는데요  \( build.gradle\)

```text
// 우선적으로 plugins 를 추가해줍니다.
 
 
plugins {
    id "org.sonarqube" version "2.6.2"
}
 
apply plugin: "org.sonarqube"
 
sonarqube {
    properties {
        property "sonar.host.url", "서버주소"
        property "sonar.sources", "src/main/java"
        property "sonar.language", "java"
        property "sonar.sourceEncoding", "UTF-8"
        property "sonar.projectKey", "smaple-project"
        property "sonar.projectName", "샘플프로젝트"
    }
}
```

### 실행 <a id="id-&#xC18C;&#xC2A4;&#xCF54;&#xB4DC;&#xC815;&#xC801;&#xBD84;&#xC11D;&#xB3C4;&#xAD6C;-&#xC2E4;&#xD589;"></a>

대략적으로 위와 같이 작성한다음 해당 타스크를 돌려주면 됩니다. 방화벽은 열려 있어야 하며 , 해당 projectKey ,projectName 이 일치 하여야합니다. \(소나큐브서버에 \) 그리고 exclusions 설정, inclusions 설정, jacoco reportPaths 설정, junit reportPaths 설정 등등도 여러가지 설정을 할 수 있는것으로 알고있어요 

그러면 일단 위에서 작성한 타스크를 실행해보죠 터미널에서 아래 와 같이 실행해 봅니다.

```text
➜  ap-sample (feature/upsourceTest) ✔ pwd
/Users/amore/IdeaProjects/ap-sample
➜  ap-sample (develop) ✔ gradle clean sonarqube
Starting a Gradle Daemon (subsequent builds will be faster)
 
// 생략
 
Deprecated Gradle features were used in this build, making it incompatible with Gradle 6.0.
Use '--warning-mode all' to show the individual deprecation warnings.
See https://docs.gradle.org/5.4.1/userguide/command_line_interface.html#sec:command_line_warnings
 
BUILD SUCCESSFUL in 20s
```

### 업로드된 정보 확인 <a id="id-&#xC18C;&#xC2A4;&#xCF54;&#xB4DC;&#xC815;&#xC801;&#xBD84;&#xC11D;&#xB3C4;&#xAD6C;-&#xC5C5;&#xB85C;&#xB4DC;&#xB41C;&#xC815;&#xBCF4;&#xD655;&#xC778;"></a>

그럼이제 위와 같은 메시지가 나오게 되는데요 그러면 이제 팀내 소나큐브 서버에 가보면 올라가 있을꺼에요.

![&#xC5C5;&#xB85C;&#xB4DC;&#xB41C; &#xC694;&#xC57D;&#xC815;&#xBCF4;](https://curtiskim.github.io/img/sonar/100.png)

대략적으로 나옵니다.  우선 전체적으로는 failed 가 나옵니다. \( test code 에서 실패한게 있어서 그렇게 나오는 것 같아요 \)

대략적으로 보게되면 크리티컬한 부분은 0건이고 코드냄새가 나는 부분이 75건으로 나옵니다. 그리고 옆에  coverage 라고 나오는부분이 바로 해당 코드의 테스트 코드에 대한 내용인데요. 전체 소스중 테스트 코드가 존재하는 부분은 15% 라는 것 입니다. \( 매우매우 낮은 수치이죠.. 반성 하겠습니다.\) 

그리고 옆에  중복 소스에 관한 내용인데 실제로 저부분은 자세하게 타고타고 들어가보면 정확한 중복까지는 못잡아주는 것 같습니다. 중복이 아닌데도 중복으로 나오는 부분도 있는 것 같아요. 실제로 해당 소스에가서 lint 를 돌려봐도 말이죠

음 저부분의 수치는 조금 더 학습이 필요 합니다.  \( 나중에 좀더 자세하게 포스팅할 기회가 있거나 공부하게 되면 해당 글에 추가적으로 정리를 해두도록 하겠습니다. 

어찌되었건 이렇게 되면 이제 팀에서 보유한 정적분석도구를 이용하여 코드 품질 테스트를 확인할 수 있게 되었습니다. \(이부분은 매번 빌드 될때 혹은 배포가 이루어질때 자동으로 올라가게 하여 팀내 공유하고 기록으로 남겨둘 수 있습니다 \)

그리고 해당 소나큐브에 올라가있는 상세 화면을 보게되면 조금더 자세하게 알수있어요

![&#xC5C5;&#xB85C;&#xB4DC;&#xB41C; &#xC0C1;&#xC138;&#xC694;&#xC57D; &#xC815;&#xBCF4;](https://curtiskim.github.io/img/sonar/101.png)

### 상세 화면 <a id="id-&#xC18C;&#xC2A4;&#xCF54;&#xB4DC;&#xC815;&#xC801;&#xBD84;&#xC11D;&#xB3C4;&#xAD6C;-&#xC0C1;&#xC138;&#xD654;&#xBA74;"></a>

그리고 코드 냄새나는 부분을 클릭해보면

어느부분인지 어떤 파일의 몇번째 라인인지 까지 정확하게 알려줍니다.

![&#xC0C1;&#xC138;&#xD654;&#xBA74;](https://curtiskim.github.io/img/sonar/102.png)

참 정확하고 좋죠. 그렇지만 여기서 주의점이 있어요. 여기서 알려주는 부분이 정확하게 코드가 냄새나는 부분이긴 하지만 이 부분은 팀에서 적용하고있는 내용과 맞는걸 수도 아닐수도있어요 그래서 이부분은 팀내의 시니어 개발자분과 함께 리뷰를 하면서 조정을 할필요가 있어 보입니다. 

그래서 해당 프로젝트의 PL 분들이나 개발 팀장님 , 시니어 분들이 적절하게 소나큐브의 레벨을 조정해주셔야 합니다.

### 기타 <a id="id-&#xC18C;&#xC2A4;&#xCF54;&#xB4DC;&#xC815;&#xC801;&#xBD84;&#xC11D;&#xB3C4;&#xAD6C;-&#xAE30;&#xD0C0;"></a>

그리고 또한 프로젝트 시작전에 우리는 코드냄새까지는 넘어가고 버그나 취약성까지는 꼭 잡고 잡고 가자!! 라는 명확한 정의가 있어야 되요. 그리고 여기서 테스트 커버리지 또한 몇퍼센트까지 허용할 것 이냐도 정의 해야합니다. 

이런부분들을 시니어 개발자 , 리더분들 개발팀장님 이러한 분들이 팀내 규칙과 문화를 만들어야 한다고 생각합니다.

그리고 제 개인적인 생각은 정적 분석도구나 , 코드 리뷰 같은 부분은 개발자를 혼내거나 하는 도구가 아니라고 생각합니다. 팀내 개발문화와 개발코드가 보다 나은 코드로 갈수있게 도와주고 팀내 개발자들도 서로서로 발전을 할수있는 계기를 만들어 줄수 있는 도구로 생각 해야지 해당 도구를 이용하여 마치 해당 도구가 정답인 것처럼 프로젝트가 진행되면 안된다고 생각합니다.

다른 분들도 소나큐브 서버를 설치하고 각자의 팀내에서 다음 프로젝트들은 앞으로 프로젝트들이 진행될때 해당 소나큐브를 적극 사용하여 프로젝트를 좀더 재밌게 진행하면 좋을 것 같아요~ ^^

jenkins 랑 머지리퀘스트를 이용한 방법을 아주

### MR 리뷰 연동 <a id="id-&#xC18C;&#xC2A4;&#xCF54;&#xB4DC;&#xC815;&#xC801;&#xBD84;&#xC11D;&#xB3C4;&#xAD6C;-MR&#xB9AC;&#xBDF0;&#xC5F0;&#xB3D9;"></a>

우선적으로 작업을 해야하는 부분은 MR 이벤트를 체크할수있는 jenkins job 를 만들어야합니다.  job 만들고 다음과같이 webhook 설정을 해주고  고급을 클릭하여 key 를 만들어줘야합니다. 

jenkins 에서 빌드 유발을 훅으로 맞춰두구 secret token 설정을 해줘야 합니다.

* build when a change is pushed to gitlab webhook

아래와같이 이제 jenkins 에서 키를 발급하면 해당키를 gitlab 에 연결해줘야해요.

![&#xAE43;&#xB7A9;&#xC124;&#xC815;](https://curtiskim.github.io/img/sonar/104.png)

대충 이렇게 해주면 연결을  해주는데요 이렇게만 연결을해주면  빌드 유발을 webhook 으로 해뒀으니

해당 그래들 타스크 만들면 clean build sonarqube\(만든타스크\) 를 순차적으로 해주면 됩니다.  \( 여기서 jenkins sonarqube for jenkins 인가 하는 그 플러그인 설치하면 아마 스캐너로 해서 연동해서  쓸수 있을 것 입니다. \) 

그러면이제 해당 MR 이벤트 변경된 소스만 분석을 하여 올려주게 됩니다.

여기까지가 이제 소나큐브에 대한 내용을 간단하게 소개 해봤구요 다음으로는 이제 린트에 대해서 정리해보려 합니다.

### Lint 소개 <a id="id-&#xC18C;&#xC2A4;&#xCF54;&#xB4DC;&#xC815;&#xC801;&#xBD84;&#xC11D;&#xB3C4;&#xAD6C;-Lint&#xC18C;&#xAC1C;"></a>

그런데 여기서 하나 더 팁을 드리자면.. 사실 이 아래 내용을 적고 싶어서 위 글을 작성하게 되었습니다. 

사실 저는 아직은  팀내에서 이거만 잘 써도 된다고 생각합니다..  \(개인적인 생각입니다요...\) 

요즘 개발팀은 대부분 intelij 를 사용하고 있습니다.  그러면 이제 거기에서 플러그인을 설치할 수 있어요   \( intelij  가 아니더라도 플러그인이 존재하는 것으로 알고 있습니다.\)

거기에 sonarLint 라는 플러그인이 존재합니다. 해당 플러그인을 설치하고 

![&#xD50C;&#xB7EC;&#xADF8;&#xC778;&#xB2E4;&#xC6B4;&#xB85C;&#xB4DC;](https://curtiskim.github.io/img/sonar/105.png)

테스트하고 싶은 코드로 가서 \( 그럼 위에서 돌린 소나큐브에 나온 냄새코드를 열어서 lint 를 돌려 보죠 .\) 

![&#xD50C;&#xB7EC;&#xADF8;&#xC778;&#xB2E4;&#xC6B4;&#xB85C;&#xB4DC;](https://curtiskim.github.io/img/sonar/105.png)

굉장히 친절하게 알려줘요 . 단계별로 알려주며  오른쪽에는 Noncompliant code example 이라고 하면서요. 여기서 해당 플러그인도 소나 큐브와 마찬가지로 lint 레벨도 조절이 가능합니다~ 

이제 진짜 포스팅이 끝났습니다. 그러면 즐거운 코딩 되세요~ 

앱개발자 분들도  위 SonarLint 쓸수있을 것 같구요 IOS 는 아마 앱개발자분들이 더 잘 알고 계시겠지만 \([SwiftLint](https://github.com/realm/SwiftLint/blob/master/README_KR.md) 를 사용하시면 더 좋을수도 있어유\)

공식 홈페이지에서 지원하는 언어는 다음과 같다고 합니다.

![&#xC9C0;&#xC6D0;&#xC5B8;&#xC5B4;](https://curtiskim.github.io/img/sonar/107.png)

감사합니다.

