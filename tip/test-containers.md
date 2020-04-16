---
description: Test Containers
---

# Test Containers

안녕하세요 평범한 개발자 김봉준입니다.

{% hint style="info" %}
테스트 컨테이너는 JUnit 테스트를 지원하는 java 라이브러리 입니다. java 개발을 할때에 테스트 코드를 컨테이너 를 이용해서 하는 방법을 소개 하겠습니다.  
**본내용은 이프런 강의중** **백기선님의 강의를 듣고 따라해본 내용입니다.**

[공식 사이트](https://www.testcontainers.org/) ,  

[공식깃헙주소](https://github.com/testcontainers)
{% endhint %}

여러가지 내용이 있지만, 이번 포스팅에서는 데이터 엑세스 통합테스트 관한 내용을 다루도록 하겠습니다. 

테스트를 돌릴때 메모리나,  목킹한 것이 아니라 도커 컨테이너로 데이터베이스를 띄운 이후 해당 테스트코드를 동작할때 데이터 베이스 컨테이너를 기동 하고 테스트 수행 종료이후 데이터 베이스 컨테이너를 종료하는 부분을 따라하는 포스팅을 해보도록 하겠습니다.

### 사용법 <a id="TestContainers-&#xC0AC;&#xC6A9;&#xBC95;"></a>

### 의존성 추가 

우선 저는 gradle 을 사용중이니 gradle 에 다음과 같은 의존성을 추가합니다.   \( 제가 이전에 프로젝트 했던 디비는 포스트그래를 사용중이여서 포스트그래 의존성도 추가 해줍니다. \) 

```text
// https://mvnrepository.com/artifact/org.testcontainers/postgresql
// testcontainers
testCompile "org.testcontainers:testcontainers:1.12.4"
testCompile "org.testcontainers:postgresql:1.12.4"
```

위와같은 의존성을 build.gradle 에 추가하면 이제 postgreSQLContainer 를 사용할 수 있습니다.  
이렇게 되었을떄  아래와 같이 사용할수 있습니다.

#### 테스트 코드 사용 <a id="TestContainers-&#xD14C;&#xC2A4;&#xD2B8;&#xCF54;&#xB4DC;&#xC0AC;&#xC6A9;"></a>

대략적으로 도커컨테이너가 구동되는지 확인 하는 내용. 

```text
@ActiveProfiles("test")
@Slf4j
@RunWith(MockitoJUnitRunner.class)
public class ContainerTest {
 
  public static PostgreSQLContainer container = new PostgreSQLContainer()
      .withDatabaseName("pimstest");
 
  @BeforeClass
  public static void setUp() {
    log.info("docker start");
    container.start();
 
  }
 
  @AfterClass
  public static void after(){
    container.stop();
  }
 
  @Test
  public void testSimplePutAndGet() {
    log.debug("===================");
  }
}
```

위와같이 코드를 작성 하였을때 보게되면  withDatabaseName 을 정해줄 수 있습니다.  
공시 가이드 문서에서 보면 자세하게 나와있어서 공식 홈페이지의 가이드를 확인 해주시면 좀더 확인이 편할꺼에요.   
우선적으로 위와 같이 작성하였을때 Test application.properties 를 작성해줘야합니다.  
spring 이 구동될때 이제 바라볼 데이터베이스를 위 코드상에서 띄운 데이터 베이스를 봐야합니다. 

그런데 이제 위와같이 띄우게 되면 랜덤한 포트로 이제 구동이 되어서 spring 설정파일이 제대로 해당 포트를 알기 어려워요 그래서 이부분을 변경 해줘야하는데 다음과 같이 작성해 줍니다.

#### application-test.yml  <a id="TestContainers-application-test.yml"></a>

```text
spring:
  datasource:
    driver-class-name: org.testcontainers.jdbc.ContainerDatabaseDriver
    url: jdbc:tc:postgresql:///pimstest
  jpa:
    hibernate:
      ddl-auto: create-drop
```

위 와 같이 설정 파일에 driver-class-name 과 ,  url 부분에 jdbc:tc:postgresql  \(:tc: 를 추가해줘야합니다.\)  
그리고 pimstest 라 db명도 동일하게 작성해주셔야 합니다.   
그러면 이제 test 코드를 구동해 보겠습니다.

### **테스트코드 구동** 

테스트코드를 수행 해보면 아래 와 같이 나오게 됩니다.

![](https://curtiskim.github.io/img/test-code/100.png)

![](https://curtiskim.github.io/img/test-code/101.png)

이제 이제 위 와같이 코드로 토커 컨테이너를 실행시키고 테스트 이후 종료 시키게 됩니다.

그런데 이때  junit 의  @BeforeClass, @AfterClass  로  start\(\) . stop\(\); 를 해줘야 하는데요 이부분을 좀더 간단하게 바꿀수 있습니다.

{% hint style="success" %}
**JUnit @ Rule / @ ClassRule** :이 모드는 테스트하기 전에 컨테이너 내부에서 데이터베이스를 시작하고 나중에 분리합니다.
{% endhint %}

그래서 위에서 작성한 test 코드를 조금 더 간략하게 줄일수 있습니다.

```text
@ActiveProfiles("test")
@Slf4j
@RunWith(MockitoJUnitRunner.class)
public class ContainerTest {
 
  @ClassRule
  public static PostgreSQLContainer container = new PostgreSQLContainer()
      .withDatabaseName("pimstest");
 
  @Test
  public void testSimplePutAndGet() {
    log.debug("===================");
  }
}
```

위와같이 이제 작성해도 동일하게 도커 컨테이너가 동작했다가 중지 됩니다.

{% hint style="warning" %}
위 샘플은 JUnit4 기준으로 작성되었습니다. 

Junit5 샘픙은 공식 사이트에 잘 나와있어요 \( 한번쯤 보시고 따라 하시면 좋을 것 같습니다.\)

그리고, 여담으로 최근 spring boot 2.2.x 이상부터는 이제 junit4 가 기본이 아니라 junit5가 기본이니 junit5 로 해보시는 것을 추천드립니다. \( 제가 테스트한 프로젝트는 spring boot 2.1.7 이여서 일단 4로 했습니다. \)

백기선님 강의에도 junit5 로 하는 방법이 나옵니다~ \( 4는 위에처럼 하면되요 \) 
{% endhint %}

자 그럼 다음으로는 JPA 랑 이제 함께 써보려고하는데요.  
우선 어떻게 하면 위 테스트를 spring boot + jpa 로 테스트해볼 수 있을까요 ? 

우선적으로 설정 yml 에는  아래 와 같이 설정을 해두었으니 이제 @entity 클레스들을  스캔해서 지웠다가 다시 테이블을 생성 하게 됩니다.

```text
jpa:
  hibernate:
    ddl-auto: create-drop
```

이제 아까 작성한 테스트 코드를 다음과 같이 수정해줍니다.

```text
@DataJpaTest
@AutoConfigureTestDatabase(replace=AutoConfigureTestDatabase.Replace.NONE)  // 여기서 이게 없을 경우 error 가 발생하는데 자세 한 이유는 아래서 설명하겠습니다.
@RunWith(SpringRunner.class)
@ActiveProfiles("test")
@Slf4j
public class ContainerTest {
 
  @ClassRule
  public static PostgreSQLContainer container = new PostgreSQLContainer()
      .withDatabaseName("pimstest");
 
  @Test
  public void testSimplePutAndGet() {
    log.debug("===================");
  }
 
}
```

위와같이 이제 @DataJpaTest 어노테이션을 사용하여 테스트 해보면 됩니다. 

그런데 이때 @DataJpaTest 어노테이션으로 테스트를 하게되면  테스트 데이터 베이스를 찾게 되어있어요  해당 어노테이션을 따라가보면 아래 와 같은 내용 이있는데요 이때  @AutoConfigureTestDatabase  어노테이션 효과로 테스트 디비를 찾게 되어있습니다.   \( 제가 잘못 알고 있는걸수 있습니다..;; 하하 \)

jpa test 슬라이스 테스트 어노테이션이 어떤걸 가지고있는지 봐보면 아래와 같습니다.

```text
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@BootstrapWith(DataJpaTestContextBootstrapper.class)
@ExtendWith(SpringExtension.class)
@OverrideAutoConfiguration(enabled = false)
@TypeExcludeFilters(DataJpaTypeExcludeFilter.class)
@Transactional
@AutoConfigureCache
@AutoConfigureDataJpa
@AutoConfigureTestDatabase
@AutoConfigureTestEntityManager
@ImportAutoConfiguration
```

어찌 되었건 이때 테스트 디비가 아니라 그냥 내가 만든 도커 컨테이너 바라보게 하고싶을때에  무시하는 설정을 해두면 됩니다.   @AutoConfigureTestDatabase\(replace=AutoConfigureTestDatabase.Replace.NONE\) 를 설정해주면되요 

그럼 다음으로 이제 도커 컴포즈로 해당 테스트 코드를 동작하는 방법까지만 알아보겠습니다.   
도커 컴포즈 파일 경로는 다음과 같아요

### 도커 컴포즈

#### src/test/resources/docker-compose.yml 을 만들어 줍니다. <a id="TestContainers-src/test/resources/docker-compose.yml&#xC744;&#xB9CC;&#xB4E4;&#xC5B4;&#xC90D;&#xB2C8;&#xB2E4;."></a>

```text

version: "3"
 
services:
  pimstest-db:
    image: postgres
    ports:
      - 5432
    environment:
      POSTGRES_DB : pimstest
      POSTGRES_USER: pimstest
      POSTGRES_PASSWORD: pimstest
    volumes:
      - ./schema.sql:/docker-entrypoint-initdb.d/1.sql
```

그리고 테스트 코드를 다음과 같이 변경해줍니다. \( 컨테이너 만드는 부분만 바꾸면 되요 \)

```text

@DataJpaTest
@AutoConfigureTestDatabase(replace=AutoConfigureTestDatabase.Replace.NONE)
@RunWith(SpringRunner.class)
@ActiveProfiles("test")
public class ContainerTest {
 
  @ClassRule
  public static DockerComposeContainer dockerComposeContainer = new DockerComposeContainer(
      new File("src/test/resources/docker-compose.yml"))
      .withExposedService("pimstest-db", 5432);
 
  @Test
  public void testSimplePutAndGet() {
    log.debug("===================");
  }
 
}
```

위와같이 PostgreSQLContainer → DockerComposeContainer 로 변경해주고 도커 컴포즈 파일 위치를 적어주고 서비스 명을 적어주면 됩니다. \( 간단하죠 ? \)

#### 그리고나서 테스트 코드를 돌려보면  이제 정상적으로 돌아가요. <a id="TestContainers-&#xADF8;&#xB9AC;&#xACE0;&#xB098;&#xC11C;&#xD14C;&#xC2A4;&#xD2B8;&#xCF54;&#xB4DC;&#xB97C;&#xB3CC;&#xB824;&#xBCF4;&#xBA74;&#xC774;&#xC81C;&#xC815;&#xC0C1;&#xC801;&#xC73C;&#xB85C;&#xB3CC;&#xC544;&#xAC00;&#xC694;."></a>

위에서 나온 방법은 junit4 입니다. 다음에 spring boot 2.2.x 버전을 사용할일이 생긴다면 junit5 관련된 내용을 올려둘께요.

### 마치며

사용해보니 간단하게 사용할 수 있는데 테스트 수행시간이 조금 더 많이 걸린다는 단점이 있는데요  1초 걸리던거 3초걸린다던지  등등 \( 컴퓨터사향에 따라 다를 수 있습니다.\) 

테스트를 도커로 구동하게되면 이제 시간은 조금더 걸릴수있으니 ci 에서만 도커 로 테스트를 돌려보면 좋을 것 같습니다. \(테스트코드 프로파일 설정을 해버려서 ... \) 

그리고 테스트  DB이니 마음대로 저장 수정 삭제도 다해봐도 될 것 같아요 \( 순차 테스트할때 등 \) 

저는그냥 db\(외부\) 가져 오는 정보는 그냥 목킹을 했었는데   \( when , thenReturn \) @Mock Repository  , @InjectMock  Service \)  데이터를 서빙해서 테스트 하곤했는데 그렇게 하지않고 도커 컨테이너를 통해서 초기데이터 밀어 넣고 해도 좋을 것 같습니다.



감사합니다. 

### 참고사항

제가 위에 예제에서는 테스트 코드를 위처럼 도커 컨테이너로 사용하지않고 메모리에 올려서 사용할 수 있습니다 \( h2\)

간략하게 설명을 드리자면 먼저 gradle 에  의존성을 다음과 같이 추가해줍니다.

```text
testImplementation("com.h2database:h2:1.4.199")
```

테스트 스콥으로 이제 h2 를 사용할 수 있는데요 이렇게 한다음에 application-test.yml 을 생성해줍니다.

```text
# TEST
spring:
  datasource:
    url: jdbc:h2:mem:test;DB_CLOSE_DELAY=-1
    username: sa
    password:
    platform: h2
    driver-class-name: org.h2.Driver
  jpa:
    hibernate:
      ddl-auto: none
    properties:
      hibernate:
        dialect: org.hibernate.dialect.H2Dialect
        show_seq: true
    database-platform: h2
    show-sql: true
    generate-ddl: false

  profiles:
    active: test
```

우선대충  위와같이 작성하게 되면 이제 테스트코드 수행시에 메모리 로 h2를 바라보게 되어있습니다.  
이렇게 되면 아까전에 설정해두었던 아래 부분을 제거 해주시면 됩니다. 

그러면 기본적으로 @DataJpaTest 어노테이션에 딸려있는 테스트 데이터베이스를 보는 설정으로 되며 우리가 테스트 데이터 베이스를 설정해두었으니 위 설정 파일에 알맞게 테스트 데이터베이스가 구동하게 되어 있습니다.

```text
@AutoConfigureTestDatabase(replace=AutoConfigureTestDatabase.Replace.NONE)
```

그리고 나서 이제 알맞게 슬라이싱 테스트를 작성해서 테스트 해보면 됩니다.\( 프로파일 설정은 test \)

```text
@DataJpaTest
@ActiveProfiles("test")
@Import(AuditorConfig::class, CreateAuditor::class, QuerydslConfiguration::class)
internal class ChannelProductTest(
    @Autowired private val repository: ChannelProductRepository
) {

  @Test
  @DisplayName("일단 전체 조회")
  @Transactional(readOnly = false)
  fun get_channel_product() {
    val list = repository.findAll()
    Assertions.assertThat(list).isNotEmpty
  }

}
```

**findAll은 테스트를 하지 않아도 되지만 일단 예시를 들려고 해두었습니다.  \(위 테스트 코드는 코틀린 입니다. \)**

감사합니다.

