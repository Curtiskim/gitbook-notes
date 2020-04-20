---
description: upsource code review
---

# upsource

안녕하세요 평범한 개발자 김봉준입니다.

{% hint style="info" %}
 좋아하는 개발자분이 계신데 그분 블로그\(참조 :  [조졸두](https://jojoldu.tistory.com/) \) 를 보면 upsource 에 대해서 나옵니다.  
그래서 저도한번  따라해보는 시간을 가져볼께요.  
  
우선 간략하게 업소스가 어떤건가하면은 젯브레인즈에서 개발한 것이며 코드리뷰를 할때 조금더 도움을 줄수있는 개발도구라고 할 수 있습니다.   
  
업소스 [공식사이트](%20https://www.jetbrains.com/help/upsource/getting-started.html)
{% endhint %}

  
일단 설치형입니다.  
다만 10명까지만 공짜에요.. 하하..  
비용이 조금 걱정이거나 팀내 규모가 조금 크다고 생각하면  그냥 깃랩의 머지리퀘스트, 깃헙의 PR을  이용해서 리뷰 문화를 만들어도 좋을 것 같습니다.  ^^\)  

그래도 한번 알아보죠!!

#### 설치 하는 방법은 매우 간단해요 공식 홈페이지가서 다운로드 받아서 zip 을 풀면 됩니다.  이부분은 따로 설명하지 않겠습니다.. \( 매우 간단하고 조금만 찾아보면 잘 나와있어서요 \)  <a id="Upsource2019.1-&#xC124;&#xCE58;&#xD558;&#xB294;&#xBC29;&#xBC95;&#xC740;&#xB9E4;&#xC6B0;&#xAC04;&#xB2E8;&#xD574;&#xC694;&#xACF5;&#xC2DD;&#xD648;&#xD398;&#xC774;&#xC9C0;&#xAC00;&#xC11C;&#xB2E4;&#xC6B4;&#xB85C;&#xB4DC;&#xBC1B;&#xC544;&#xC11C;zip&#xC744;&#xD480;&#xBA74;&#xB429;&#xB2C8;&#xB2E4;."></a>

```text
다운 받은 zip 파일을 압축을 해제
/bin 폴더의 upsource.sh or upsource.bat 파일을 자기의 os 에 맞게 실행
화면이 나오면 admin 계정 만든다 .
next → next → next 클릭한다.
```

### 설치

![](https://curtiskim.github.io/img/upsource/100.png)

대충 이제 설치를 했다고 치면 깃헙이나 각 회사에서 사용하고 있는 버전관리랑 연결 할 수 있어요.~

저희는 깃랩을 사용하고있는데 제가 토큰 발급 권한이 없어서 .. 일단 계정으로 연동이 가능하니 제 깃랩 계정으로 연동을 해두었습니다.

\( 초기 셋팅중. \)

![](https://curtiskim.github.io/img/upsource/101.png)

이제 간략하게 화면을보게되면

![](https://curtiskim.github.io/img/upsource/102.png)

위와같은 화면이 나오게 됩니다. 이제 그럼 connect to a github project 를 눌러서 우리의 깃랩 주소를 적으면 되요~  \( 물론 계정정보도 적어줘야합니다. \) 

그렇게 해서 팀에서 관리하는 프로젝트중 제가 샘플로 아래 4개를 임의로 받아왔습니다. 

![](https://curtiskim.github.io/img/upsource/103.png)

이제 위와 같이 이렇게 보게되면 여러가지를 할 수 있습니다.   
우선적으로 위 upsource 에서 깃랩과 싱크를 맞추는 시간을 정해줄수있는데요 기본설정은 \(300초 \) 5분입니다.   
하지만 지금은 그냥 테스트용도 이고 샘플이니 각 프로젝트별로  다음과 같이 설정해두었습니다. 

| 프로젝트명 | 연동 시간 \( 초 \) |
| :--- | :--- |
| pims | 3000 |
| android-ap | 4000 |
| eap | 5000 |
| android-emt | 6000 |

이렇게만 해둬도 재밌는 것을 확인 할 수 있습니다.

### 1. 브랜치별 커밋 로그

![](https://curtiskim.github.io/img/upsource/104.png)

간단하게 위에서 보이는것처럼 커밋 로그도 확인할 수 있으며 해당 커밋의 소스까지 확인해볼 수 있습니다.

### 2. 통계 

![](https://curtiskim.github.io/img/upsource/105.png)

* 통계 보게되면 월별 로 확인할 수 있으며 개발자 활동순으로도 정렬이 가능합니다. 
* 기간별로 활동한 사용자를 볼 수 도 있습니다. 

### 3. project treemap 

![](https://curtiskim.github.io/img/upsource/106.png)

* 파일의 종류의 크기의 따라서 Tree Map 을 보여주는 화면입니다.
* 특정 프로젝트가 어떠한 종류의 파일이 많은지 시각화해서 보여줍니다.

### 4 file history

![](https://curtiskim.github.io/img/upsource/107.png)

* 파일 히스토리를 나타내며 그래프 위쪽으로 갈수록 한 파일을 여러번 고치게 된 파일이 됩니다.
* 이러한 그래프로 유추해볼때 어느 파일을 제일 많이 수정하게 되는지 확인도 될 수 있습니다.   \( 몇일간 몇번 수정되었는지 알수있습니다. \) 

### 4. code review

![](https://curtiskim.github.io/img/upsource/108.png)

* 코드 리뷰에 대한 내용을 알려주는데요. 
* 이부분은 이제 MR과 연동 하면서 더 자세하게 설명하도록 하게습니다.

### 나만의 대시보드 만들기

![](https://curtiskim.github.io/img/upsource/109.png)

대시보드를 만들어서 보고싶은 내용을 위와같이 위젯 형태로 만들어서 나만의 대시보드를 만들어 둘수 있습니다.

### Review

**여기서 제가 위 업소스를 사용하려고하는 이유가 바로 리뷰인데요.  이 리뷰 부분은 다음 포스팅으로 넘어가도록 하겠습니다.**

코드리뷰는 개발 품질을 높이는데 사용할수있고 팀내 문화가 정착되어야 한다고 생각합니다.  개인적인 생각으로는 테스트 코드 , 정적 분석 보다도 리뷰 문화가 먼저 생겨야 된다고 생각합니다.   
리뷰를 하게되면 좋은점은로는  여러가지 좋은점이 있다고 생각합니다. 

* 지식공유 
* 품질
* 역량 향상

다만 , 비용이 듭니다. 여기서 비용이란 , 개인의 **시간 자원**이 들 수 있습니다.  \( 여기서 개인적인 생각은 개발 일정을 잡을때에 리뷰까지 하는 시간으로 잡아야 된다고 생각해요. 리뷰도 품질향상을 위한 것이니 업무라고 생각합니다. \)

upsource 를 사용하거나 gitlab 의 MR 기능을 사용해서 리뷰를 한다면 따로 회의를 잡을 필요고 없어 시간 절약이 되며 의사소통도 댓글로 하게 되니 시간 비용이 많이 절약될 수있다고 생각합니다. \( 회의리뷰나 기타 다른 리뷰에 비해 도구의 장점 \) 

* 코드리뷰는 개인의 시간 비용이 들지만 나중에라도 팀내 문화로 자리잡으면 좋을 것 같습니다.
* upsource 는 메신저 역할도 대신할 수 있어 의사소통에도 유용합니다. \(IDE 플러그인 설치시 \)

upsource는 코드 리뷰를 조금더 개발자스럽고 번거롭지 않게 할수있게 도와줍니다. 

### Upsource 사용비용 \(연간비용\)

| 인원 | 가격 |
| :--- | :--- |
| 10명 이하 | $0 |
| 25명 이하 | $1,300 |
| 50명 이하 | $2,500 |
| 100명 이내 | $4,500 |
| 250명 이내 | $9,000 |

소개한 upsource가 월별통계 및 기타 대시보드로 확인할 수 있는 내용들이 많은데 

upsource 는 개발팀을 위한 공동 작업 도구로 사용 되어야 하며,  개인의 역량평가나 개인을 지적하기 위한 도구로 이용되지 않는게 좋을 것 같습니다. 

### upsource 소개 에 이어 이번에는 upsource 활용편에 대해 정리해보도록 하겠습니다.

{% hint style="info" %}
이부분은 이제 Upsource 를 활요하여 코드리뷰에 어떻게 활용할지에 대해서 정리해보는 시간을 가지도록 하겠습니다.
{% endhint %}

* 특정 브랜치 \( review 를 남기고 싶은 곳에 가서 \)
* 여기서 이제 cretae Merge review 만들어 볼수있어요 다음과 같은 화면에서 ~ 

![](https://curtiskim.github.io/img/upsource/110.png)

이제 저렇게 만들게 되면 AS-CR-2라고 나오는데 저희가 네이밍 규칙을 프로젝트 셋팅에 두었는데요 이부분이 AS-CR-{} 로 해두어서 이제 저렇게 만들어집니다.

![](https://curtiskim.github.io/img/upsource/111.png)

여기서 주위 할점은 위에처럼 타겟 브랜치를 MR 타겟이랑 맞춰서 해줘야 합니다.  
안그렇게 되면 default 브랜치가 나타나게 됩니다.

![](https://curtiskim.github.io/img/upsource/112.png)

이제 그럼 위 화면처럼 merge review 를 만들고 내용을 적어서 저장을 누르게 되면 이제 리뷰가 시작됩니다 .

![](https://curtiskim.github.io/img/upsource/113.png)

* 이제 그럼 다음과 같은 화면에서 
* reviewers 를 등록해줍니다 \( amore 등록 \)

이제 그럼 upsource는 amore 로 접속을 하고  

인텔리 J 를 접속합니다. 

* intelij 에 플러그인중 upsource 플러그인을 설치하고 주소를 적어주고 test connection 을 눌러주고 해당 프로젝트에 접근하게되면 이제  IDE 와 upsource 가 연동이 완료 됩니다.

![](https://curtiskim.github.io/img/upsource/114.png)

연동을 하면 브라우저에 확인 창이 나오는데 클릭하게되면 이제 성공 됩니다.

![](https://curtiskim.github.io/img/upsource/115.png)

### 그러면 이제 upsource 에서 해당 소스부분에 대한 리뷰를 시작해보죠!!!  <a id="Upsource-Review-&#xADF8;&#xB7EC;&#xBA74;&#xC774;&#xC81C;upsource&#xC5D0;&#xC11C;&#xD574;&#xB2F9;&#xC18C;&#xC2A4;&#xBD80;&#xBD84;&#xC5D0;&#xB300;&#xD55C;&#xB9AC;&#xBDF0;&#xB97C;&#xC2DC;&#xC791;&#xD574;&#xBCF4;&#xC8E0;!!!"></a>

![](https://curtiskim.github.io/img/upsource/116.png)

* 위와 같이 이제 간략한 리뷰를 작성  \( 리뷰 부분은 리뷰를 위해서 일부러 소팅하는부분을 구현했다가 람다로 변경하고 comparator int 로 변경하는 부분을 예시로 만들었습니다. \)
* 다음과 같이 작성하게되면이제 upsource 에서 아래 화면처럼 확인이 가능합니다.

![](https://curtiskim.github.io/img/upsource/117.png)

* 확인 가능한부분 
* 한번더 작성해보죠 

![](https://curtiskim.github.io/img/upsource/118.png)

* 한번더 작성한 부분 

#### 여기까지가 이제 리뷰어가 upsource 에서 리뷰를 시작한 부분입니다. \( 다음은 이제 소스 개발을 하고있는 개발자 입장에서의 화면을 확인해보겠습니다. \)  <a id="Upsource-Review-&#xC5EC;&#xAE30;&#xAE4C;&#xC9C0;&#xAC00;&#xC774;&#xC81C;&#xB9AC;&#xBDF0;&#xC5B4;&#xAC00;upsource&#xC5D0;&#xC11C;&#xB9AC;&#xBDF0;&#xB97C;&#xC2DC;&#xC791;&#xD55C;&#xBD80;&#xBD84;&#xC785;&#xB2C8;&#xB2E4;.(&#xB2E4;&#xC74C;&#xC740;&#xC774;&#xC81C;&#xC18C;&#xC2A4;&#xAC1C;&#xBC1C;&#xC744;&#xD558;&#xACE0;&#xC788;&#xB294;&#xAC1C;&#xBC1C;&#xC790;&#xC785;&#xC7A5;&#xC5D0;&#xC11C;&#xC758;&#xD654;&#xBA74;&#xC744;&#xD655;&#xC778;&#xD574;&#xBCF4;&#xACA0;&#xC2B5;&#xB2C8;&#xB2E4;.)"></a>

개발자 A는 이제 intelij 에서 개발을 하던 도중에 팀 동료가 upsource 에서 위와같이 리뷰를 남겨주면 개발하는 도구\(intelij\)에서 다음과 같이 알람 및 메시지가 나오게 됩니다. 

![](https://curtiskim.github.io/img/upsource/119.png)

![](https://curtiskim.github.io/img/upsource/120.png)

* 이렇게 개발툴 왼쪽 상단에서 누가 어떤식으로 자기에게 남겼는지 확인이 됩니다. 
* 여기서 중요한건 해당 툴에서 바로 피드백을 줄수있습니다  \(댓글 기능 가능 \) 이모티콘,  따봉기능이 가능합니다.
* 물론 관리자도 바로바로 피드백이 가능합니다.

![](https://curtiskim.github.io/img/upsource/121.png)

* 그리고 바로 오른쪽 하단부분에서도 메신저처럼  알람이 옵니다 .
* 여기서도 답변이 바로바로 가능합니다.

![](https://curtiskim.github.io/img/upsource/122.png)

다음과 같이 답변을 달게되면 이제 메신저처럼 IDE 에서 바로 답변을 달수 있게 됩니다. \( 쉽죠~ \)

![](https://curtiskim.github.io/img/upsource/123.png)

바로 연동된 부분.



{% hint style="warning" %}
**이제 코드 리뷰가 따로 회의실을 잡지 않아도 되며 메일로 피드백을 받지 않아도 되고 바로 개발툴에서 확인이 가능하고 ,관리자는 upsource 에서 개발 현황 및 개발 코드를 리뷰해줄수 있게 됩니다.** 

**되면 개발자는 메신저 하듯이 코드 리뷰를 하기 때문에 코드리뷰에 거부감이 조금 덜 할수 있으며 , 코딩을 하는 재미를 더 가질수 있을 것 같습니다.** 

**관리자는 이제 upsource 통해 팀내 프로젝트 현황을 파악할 수 있습니다. \(1부에서 확인한 통계등등 \)**
{% endhint %}

####  <a id="Upsource-Review-&#xADF8;&#xB9AC;&#xACE0;&#xC774;&#xC81C;&#xC774;&#xB807;&#xAC8C;&#xB9AC;&#xBDF0;&#xAC00;&#xC313;&#xC774;&#xAC8C;&#xB418;&#xBA74;&#xD574;&#xB2F9;&#xD504;&#xB85C;&#xC81D;&#xD2B8;&#xC758;&#xB9AC;&#xBDF0;&#xC5D0;&#xB300;&#xD55C;&#xD1B5;&#xACC4;&#xB97C;&#xD655;&#xC778;&#xD574;&#xBCFC;&#xC218;&#xC788;&#xC5B4;&#xC694;"></a>

####  <a id="Upsource-Review-&#xADF8;&#xB9AC;&#xACE0;&#xC774;&#xC81C;&#xC774;&#xB807;&#xAC8C;&#xB9AC;&#xBDF0;&#xAC00;&#xC313;&#xC774;&#xAC8C;&#xB418;&#xBA74;&#xD574;&#xB2F9;&#xD504;&#xB85C;&#xC81D;&#xD2B8;&#xC758;&#xB9AC;&#xBDF0;&#xC5D0;&#xB300;&#xD55C;&#xD1B5;&#xACC4;&#xB97C;&#xD655;&#xC778;&#xD574;&#xBCFC;&#xC218;&#xC788;&#xC5B4;&#xC694;"></a>

#### 그리고 이제 이렇게 리뷰가 쌓이게 되면 해당 프로젝트의 리뷰에 대한 통계를 확인해볼수있어요  <a id="Upsource-Review-&#xADF8;&#xB9AC;&#xACE0;&#xC774;&#xC81C;&#xC774;&#xB807;&#xAC8C;&#xB9AC;&#xBDF0;&#xAC00;&#xC313;&#xC774;&#xAC8C;&#xB418;&#xBA74;&#xD574;&#xB2F9;&#xD504;&#xB85C;&#xC81D;&#xD2B8;&#xC758;&#xB9AC;&#xBDF0;&#xC5D0;&#xB300;&#xD55C;&#xD1B5;&#xACC4;&#xB97C;&#xD655;&#xC778;&#xD574;&#xBCFC;&#xC218;&#xC788;&#xC5B4;&#xC694;"></a>

![](https://curtiskim.github.io/img/upsource/124.png)

#### 소스 코드 리뷰에 대한 upsource 참고 화면  <a id="Upsource-Review-&#xC18C;&#xC2A4;&#xCF54;&#xB4DC;&#xB9AC;&#xBDF0;&#xC5D0;&#xB300;&#xD55C;upsource&#xCC38;&#xACE0;&#xD654;&#xBA74;"></a>

![](https://curtiskim.github.io/img/upsource/125.png)

여기에서 바로바로 리뷰를 남길 수 도있습니다 create merge review 를 하지 않아도..

그밖에도 Jira 연동 직접 깃렙에 MR 올려서 리뷰 받기 등 다양한 기능이 있는 것 같아요 이런 다양한 기능들은 팀내 리뷰 문화가 정착되면 이제 사용해보면서 하나하나 익혀 가면 좋을 것 같습니다. 



이제 그러면 재밌는 개발 하시고 재밌는 리뷰 시간을 가지시면 될 것 같아요

감사합니다.

