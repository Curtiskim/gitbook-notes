---
description: spring boot 와 간단하게 응용해보는 kotlin
---

# Kotlin 응용 in Spring Boot

kotlin 1-3 시리즈는 kotlin 처음 접해본 제가 그냥 kotlin 기초를 정리해 봤다면,  이번 포스팅은 웹개발에 코틀린을 어떻게 처음 적용해볼까? 의 기초 라고 보시면 좋을 것 같습니다.

{% hint style="info" %}
이번내용은 **백기선**님 인프런 ㄱ강의 내용을 토대로 만들어봤습니다.
{% endhint %}

우선 샘플프로젝트는 web application 샘플로 만들도록 하겠습니다.

샘플에 사용된 부분

java, kotlin, gradle, jpa , h2 , spring-boot , spring security , thymeleaf

spring boot 를 사용한 이유는 스프링 부트로 시큐리티 설정시 SecurityFillterAutoConfiguration 으로 스프링 시큐리티 필터 프록시가 자동으로 설정되어 따로 설정을 안해줘도 됩니다.

프로젝트를 만들기 전에 간단하게 스프링 시큐리티에서의 인증은 다음과 같습니다.

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#xB3C4;&#xC6C0;&#xB9D0;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        <p></p>
        <p>&#xC2A4;&#xD504;&#xB9C1; &#xC2DC;&#xD050;&#xB9AC;&#xD2F0;&#xC5D0;&#xC11C;
          &#xC778;&#xC99D;&#xC740; AuthenticationManager&#xAC00; &#xD569;&#xB2C8;&#xB2E4;.</p>
        <ul>
          <li>&#xC778;&#xC790;&#xB85C; &#xBC1B;&#xC740; Authentication&#xC774; &#xC720;&#xD6A8;&#xD55C;
            &#xC778;&#xC99D;&#xC778;&#xC9C0; &#xD655;&#xC778;&#xD558;&#xACE0; Authentication
            &#xAC1D;&#xCCB4;&#xB97C; &#xB9AC;&#xD134;&#xD55C;&#xB2E4;.</li>
          <li>&#xBCA8;&#xB9AC;&#xB370;&#xC774;&#xC158; &#xCCB4;&#xD06C;&#xD6C4; &#xC5D0;&#xB7EC;&#xB97C;
            &#xB118;&#xAE38;&#xC218; &#xC788;&#xC74C;</li>
        </ul>
        <p>&#xC778;&#xC790;&#xB85C; &#xBC1B;&#xC740; Authentication</p>
        <ul>
          <li>&#xC0AC;&#xC6A9;&#xC790;&#xAC00; &#xC785;&#xB825;&#xD55C; &#xC778;&#xC99D;&#xC5D0;
            &#xD544;&#xC694;&#xD55C; &#xC815;&#xBCF4;</li>
          <li>Authentication
            <ul>
              <li>Principal: &#x201C;curtiskim@naver.com&#x201D;</li>
              <li>Credentials: &#x201C;1234&#x201D;</li>
            </ul>
          </li>
        </ul>
        <p>&#xC720;&#xD6A8;&#xD55C; &#xC778;&#xC99D;&#xC778;&#xC9C0; &#xD655;&#xC778;</p>
        <ul>
          <li>&#xC0AC;&#xC6A9;&#xC790;&#xAC00; &#xD654;&#xBA74;&#xC5D0;&#xC11C; &#xD3FC;&#xC73C;&#xB85C;
            &#xBCF4;&#xB0B8; password&#xAC00; &#xC6B0;&#xB9AC;&#xAC00; &#xC55E;&#xC73C;&#xB85C;
            &#xAD6C;&#xD604;&#xD560; UserDetailsService&#xB97C; &#xD1B5;&#xD574; &#xC77D;&#xC5B4;&#xC628;
            UserDetails &#xAC1D;&#xCCB4;&#xC5D0; &#xB4E4;&#xC5B4;&#xC788;&#xB294; password&#xC640;
            &#xC77C;&#xCE58;&#xD558;&#xB294;&#xC9C0; &#xD655;&#xC778;</li>
          <li>&#xBCA8;&#xB9AC;&#xB370;&#xC774;&#xC158; &#xCCB4;&#xD06C;&#xD6C4; &#xC5D0;&#xB7EC;&#xB97C;
            &#xB118;&#xAE38;&#xC218; &#xC788;&#xC74C;</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>New Project  

우선적으로 IDE에서 이제 신규 프로젝트르 만들어줍니다. \( Spring Initializr \)

![](http://wiki-digit.amorepacific.com:8090/download/attachments/1220862/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-02-06%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.53.58.png?version=1&modificationDate=1580954069000&api=v2)

group , artifact 설정은 편하신데로 해주시면 됩니다.  \( 아래 참조 \) 

![](http://wiki-digit.amorepacific.com:8090/download/attachments/1220862/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-02-06%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.56.19.png?version=1&modificationDate=1580954245000&api=v2)



![](http://wiki-digit.amorepacific.com:8090/download/attachments/1220862/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-02-06%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.59.53.png?version=1&modificationDate=1580954429000&api=v2)

이제 위와 같이 설정을해주면 이제 gradle 의존관계에 들어가고 , 코틀린을 사용할 기본적인 셋팅이 설정됩니다

### 그래들 의존성 부분 

```text
dependencies {
  implementation("org.springframework.boot:spring-boot-starter-security")
  implementation("org.springframework.boot:spring-boot-starter-thymeleaf")
  implementation("org.springframework.boot:spring-boot-starter-web")
  implementation("com.fasterxml.jackson.module:jackson-module-kotlin")
  implementation("org.jetbrains.kotlin:kotlin-reflect")
  implementation("org.jetbrains.kotlin:kotlin-stdlib-jdk8")
  implementation("org.springframework.boot:spring-boot-starter-data-jpa")
 
  runtimeOnly("com.h2database:h2")
 
  developmentOnly("org.springframework.boot:spring-boot-devtools")
  testImplementation("org.springframework.boot:spring-boot-starter-test") {
    exclude(group = "org.junit.vintage", module = "junit-vintage-engine")
  }
  testImplementation("org.springframework.security:spring-security-test")
}
```

spring boot java main 메소드와 비슷하게 생긴 application.kt 이 생성됩니다.

```text

@SpringBootApplication
class Application
 
fun main(args: Array<String>) {
  runApplication<Application>(*args)
}
```

이제 시큐리티에서 사용할 Account entity 를 만들어줍니다. \( src &gt; main &gt; kotlin \)

```text
@Entity
data class Account(
 
    @Id @GeneratedValue
    var id: Long? = null,
    var email: String,
    var password: String,
 
    @Enumerated(EnumType.STRING)
    var roles: MutableSet<AccountRole>,
 
    var createDate: LocalDateTime = LocalDateTime.now()
 
) {
 
  fun encodePassword(passwordEncoder: PasswordEncoder) {
    this.password = passwordEncoder.encode(this.password)
  }
 
  fun getAuthorities(): User {
    return User(
        this.email, this.password,
        this.roles.stream().map { role -> SimpleGrantedAuthority("ROLE_$role") }.collect(Collectors.toSet())
    )
  }
 
}
```

* 아이디 , 이메일 , 패스워드 , 권한 , 등록일  정도를 가지는 필드를 만들어 줍니다. 
* kotlin의 data 클래스로 만들어준 이유는 set,get  hashCode equals , tostring 을  다 만들어주는 녀석이라 entity 에는 data class 로 선언 하였습니다. 
* encodePassword\(\)  : 추가적으로 패스워드는 인코딩 할수있도록 encodePassword 를 사용해서 인코딩하여 등록해줍니다 \( spring passwordEncoder 기본은 bcrypt 암호화 룰을 사용하는것으로 알고있습니다 \)  이부분도 원하는 알고리즘으로 변경 가능 합니다. 
* getAuthorities\(\) : spring security 에서 사용하는 UserDetail 타입인 User 를 전달하는 함수를 하나 만들어 줍니다. 
* 조금더 자세한 설명은 아래에 추가해두도록 하겠습니다. 

권한 관련 이넘도 생성해줍니다. AccountRole

```text
enum class AccountRole {
  ADMIN, USER
}
```

Account Entity 를 만들었으니 이제 account repository 를 만들어 볼까요

```text
interface AccountRepository : JpaRepository<Account, Long> {
  fun findByEmail(email: String): Account
}
```

저는 이번 샘플에서는 JPA 를 사용할 예정으로 JPAJpaRepository 를 상속받은 interface 를 만들어 줍니다.  

* jpa 에서 지원해주는 findBy절을 이용해서  email를 가지고 h2 데이터 베이스에서 account 객체를 하나 가져오는 함수를 추가하였습니다. 

 여기서 여담으로 jpaRepository 를 구현하는 구현체를 만들지 않아도 interface 만으로 여러가지 편리한 기능을 할수 있는데요  이 이유에대해서 간략하게 설명을 드리자면 spring data jpa 에서 이 구현체를 이미 만들어서 빈으로 등록해서 사용할 수 있습니다.

해당 Bean은 SimpleJpaRepository&lt;T , ID&gt; 인데요 나중에 추가적으로 이부분에 대해서 찾아보셔서 학습하시면 더 좋을 것 같아요.

이제 는 Account 를 저장할수있는 서비스를 하나 만들어 줍니다. 

```text
@Service
class AccountService(private val accountRepository: AccountRepository,
                     private val passwordEncoder: PasswordEncoder) {
 
  @Transactional
  fun saveAccount(account: Account): Account {
    account.encodePassword(passwordEncoder)
    return accountRepository.save(account)
  }
 
}
```

* 여기서 주입을 받는 부분이 조금 다를수 있습니다. java의 경우는  @Autowired 로 주입을하거나 ,  setter inject 을 하거나  constructor 에서 주입을 해주거나 해주면 되는데요. kotlin 에서도 이와 비슷하게 해줄수 있습니다 . 
* 저는 여기서 생성자 주입을 하게되었구요 \( val accountRepository \) 
* 주입받는 부분은 passwordEncoder , 와 accountRepository 입니다. 여기서 repository 는 위에서 만들어 두었으니  passwordEncoder를 이제 spring Bean 으로 설정해주는 부분을 만들어 보겠습니다 .
* 저장하는부분은 JPA 를 이용하며 , 저장 전에 password를 encoder를 이용하여 인코딩 해주는 부분바께 없어요. 

PasswordEncoder 부분 Bean 등록

```text
@Configuration
class BeanConfig {
 
  @Bean
  fun passwordEncoder(): PasswordEncoder {
    return PasswordEncoderFactories.createDelegatingPasswordEncoder()
  }
}
```

DelegatingPasswordEncoder 를 만들어 빈으로 등록해줍니다.

자 이제 준비가 거의 끝나 가는데요. \( entity 생성 ,repository 생성 , sevice 생성 passwordEncoder , \) 이제 브라우저 주소와 매핑 할 controller를 만들어 줍니다.

### Controller 생성.

```text
@Controller
class AccountController {
  @GetMapping("/")
  fun viewIndex(request: HttpServletRequest): String {
    return "index"
  }
 
  @GetMapping("/view/success")
  fun viewSuccess(request: HttpServletRequest): String {
    return "success"
  }
}
```

* 루트로 온 요청을 처리하는 /  mapping 과  로그인 성공시 넘겨줄 /view/success 매핑을 만들어 줍니다.
* 여기서 : String 으로 html 파일명을 매핑해준다. thymeleaf 를 사용할려고 했는데 그냥 html 만 만들었습니다. 

그럼 이제 중요한 spring security 설정 부분을 보겠습니다.

```text
@EnableWebSecurity
class SecurityConfig(private val customUserDetailService: CustomUserDetailService,
                     private val passwordEncoder: PasswordEncoder) : WebSecurityConfigurerAdapter() {
 
  override fun configure(auth: AuthenticationManagerBuilder) {
    auth
        .userDetailsService(customUserDetailService)
        .passwordEncoder(passwordEncoder)
  }
 
  override fun configure(http: HttpSecurity) {
    http.anonymous()
        .and()
        .formLogin()
        .successForwardUrl("/view/success")
        .and()
        .authorizeRequests()
        .anyRequest().authenticated()
 
  }
 
  override fun configure(web: WebSecurity) {
    web.ignoring().requestMatchers(PathRequest.toStaticResources().atCommonLocations())
  }
 
}
```



* 인증관리자빌더를 오버라이드 합니다.
  * 인증 관리자 설정에서 생성자로 주입받은 customUserDetailService 를 넘겨주는데  여기서 CustomUserDetailService 는 spring security 의 userDetailService interface 를 구현한 녀석이다. 별건없고 loadUserbyUserName 만 오버라이드 했습니다.
  * 우리가 빈으로 등록한 passwordEncoder 셋팅해 줍니다. 
  * customUserDetailSercie 에서는  위에 account entity 를 만들때 만들어둔  \(spring security User 리턴하는\) 함수를 호출합니다.  \( getAuthorities\(\) \) 
  * 스프링 시큐리티는 Principal 이라는 개념이 있는데 여기서 우리가 넘겨주는 UserDetailService 에서 넘겨주는 User 객체가 바로 Principal 이라고 할수 있습니다.
  * 여기에서 인가에 해당하는 부분 \( 권한 \) GrantAuthority 도 확인할수있는데  이부분도 principal 에 있다.  그러니까 우리가 스프링 시큐리티에서 인증을 받고 principal 이 생길때 그 부분에서 권한부분도 알 수 있는 부분이 바로 이 부분 입니다. 
  * 스프링 시큐리티는 인증 부분과 인가 부분이 중요한데 좀더 자세한 내용은 검색을 통해서 알아 봅시다.
* HttpSecurity 를 오버라이드 합니다.
  * 여기서는 formLogin\(\) 을 사용합니다.
  * 성공시에 이동할 successForwardUrl 을 지정해 줍니다.
  * authorizeRequests\(\) 어떤 리퀘스트가 와도 인증을 받을수 있도록 설정합니다.
* WebSecurity 를 오버라이드 합니다. 
  * 여기서 우리는 정적 리소스에 관하여 ignoring을 해줍니다. \( spring boot 가 제공하는 PathRequest 를 사용하여 정적 리소스 요청은 스프링 시큐리티 필터를 사용하지 않게 함 \) 
  * static Resources 부분을 위 와 같이 webSecurity 에서 위와 같이 설정해주었는데  pims 를 개발할때 필자는 httpSecurity 에 antMatchers 설정을 이용하여 permitAll\(\) 설정을 주었습니다.
    * 여기서 참고로 적어둔다면 webSecurity 부분에서 설정을 하게되면 spring security fillter 부분을 0개로 인식해서 fillter 를 타지 않게 됩니다.  
      * 하지만 httpSecurity 설정으로 permitall 설정을 해준다면 spring security filter chain에 엮인 15개 를 순차적으로 검증 받게 됩니다.  \(필터체인에 엮인 검증을 다 하지 않아도 되니 web에서 해주는게 좀더 이득.\)
    * 참고로 백기선님의 강의를 들어보면 동적인것은 httpSecurity 에 설정해주고 그게 아닌 정적 리소스 자원을 설정해주는건 webSecurity 를 사용하여 ignoring 해주는게 이득이라고 합니다. 



### 스프링 시큐리티가 제공하는 필터 목록

1. WebAsyncManagerIntergrationFilter → 스프링 시큐리티는 쓰레드 로컬을 사용하고있습니다. \( 같은 쓰래드내에서만 자원을 공유 \) 하지만 해당 필터에서 쓰레드가 다르더라도 Callarble로 쓰레드 생성할시 같은 Principal 을 공유 할수있습니다. service @async 부분은 별도의 설정을 해줘야함 \( propagation , isolation \)
2. SecurityContextPersistenceFilter → 기존 전략인 http session 캐시에 Authentication 인증 을 공유하여 다시 접근할때 재로그인 요청을 받지 않는 부분입니다 \( pims 구축시 http session 대신 redis 로 변경하여 사용하였습니다 \( 이중화 등등 이유로 \)
3. HeaderWriterFilter
4. CsrfFilter
5. LogoutFilter
6. UsernamePasswordAuthenticationFilter → 폼인증을 처리하는 부분 여기서 폼인증을 받고 나서 SecurityContextHoller 에 셋팅을 해줍니다.
7. DefaultLoginPageGeneratingFilter
8. DefaultLogoutPageGeneratingFilter
9. BasicAuthenticationFilter
10. RequestCacheAwareFtiler
11. SecurityContextHolderAwareReqeustFilter
12. AnonymouseAuthenticationFilter
13. SessionManagementFilter
14. ExeptionTranslationFilter → 인증 인가 관리
15. FilterSecurityInterceptor → 인가 부분을 처리하고 위에 익셉션 부분으로 보낸다.

다음으로는 이제 구동전에 account 가 있어야하니 우리가 spring boot 메인 메소드에서 실행하는 runner를 오버라이드 해서 구동직 후 바로  Account 를 생성 해보도록 하겠습니다. 

```text
@Configuration
class BeanConfig {
  ...
   
  @Bean
  fun applicationRunner(): ApplicationRunner {
    return object : ApplicationRunner {
      @Autowired
      lateinit var accountService: AccountService
 
      @Throws(Exception::class)
      override fun run(args: ApplicationArguments) {
        val admin = Account(null, "100m8cho@naver.com", "1234", mutableSetOf(AccountRole.ADMIN, AccountRole.USER))
        accountService.saveAccount(admin)
      }
    }
  }
 
 
  ...
}
```

여기서는 코틀린의 lateinit 이라는게 사용되는데요. 이부분은 lazy inject 이라고 볼수있는데요. kotlin 에는 null 에 민감하죠 . ? 가 없는 변수타입은 초기화를 할때 null 이 올 수 없습니다.  여기서 lateinit 부분이 바로 추기화를 늦추는 것 입니다.  \( null 초기화 할 수 없습니다. \)   


* 참고로 lateinit 은 kotlin의 var 변수선언에만 사용할수 있으며 kotlin lazy 초기화는 val 변수에만 사용할수 있습니다. \(  str : String by lazy \`{초기화}\`
* java와 다르게 acccount 를 만들때 kotlin 스럽게 NEW 키워드가 없는 것을 확인할 수 있습니다.  
* entity id 인 부분은 acount entity 설정할때 ? null 이 가능하도록 만들었으니 id 는 jpa의 Id 생성룰에 위임합니다. \( @GeneratedValue 기본설정 auto\)
  * 참고로 jpa  insert update 둘다 save 를 사용하는데요.  여기서 jpa 가 update 인지 , insert 인지 판단하는기준은 위에서 잠깐 언급한 spring data jpa 구현체에 나와 있습니다.   \( entityTinformation.isNew  \) 여기서 객체가 null 이거나 primitive type 인 경우는 \( int , long ... \)  0이면 새로운 객체라고 판단합니다. 
  * 자세한건 jpa 를 검색해보면 알 수 있어요 

여기서 이제 위와 같이 만든다음. html 을 알맞게 만들어주고 이제 localhost:8080 을 호출하면 이제  , id, pwd 를 받는 화면을 만나볼수있어요~ 

그러면 이제 아주 간단한 인증 구현은 끝났습니다.  여기서 URL 별로 인가 부분을 설정하는 부분은 다음 포스팅으로 하도록 하겠습니다.

spring security 에 아키택쳐에 관한 부분과 조금더 자세한 내용을 확인해보고 싶으시다면 아래 URL을 참고해주시면 됩니다.

[https://spring.io/guides/topicals/spring-security-architecture](https://spring.io/guides/topicals/spring-security-architecture)

[https://docs.spring.io/spring-security/site/docs/5.1.5.RELEASE/reference/htmlsingle/](https://docs.spring.io/spring-security/site/docs/5.1.5.RELEASE/reference/htmlsingle/)

위 사이트에 방문하면 볼수있는 web security filter 에 관련된 그림들.

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p></p>
        <p>web security</p>
      </th>
      <th style="text-align:left">
        <p></p>
        <p>
          <img src="http://wiki-digit.amorepacific.com:8090/download/attachments/1220862/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-02-06%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%203.14.40.png?version=1&amp;modificationDate=1580969708000&amp;api=v2"
          alt/>
          <img src="http://wiki-digit.amorepacific.com:8090/download/attachments/1220862/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-02-06%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%203.14.45.png?version=1&amp;modificationDate=1580969709000&amp;api=v2"
          alt/>
        </p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>Test Code   

이제 위에서 만든 간단한 테스트 코드를 구현해 봅시다.

```text
@SpringBootTest
@AutoConfigureMockMvc
@DisplayName("Mock Mvc Test")
internal class AccountControllerTest {
 
  @Autowired
  lateinit var mockMvc: MockMvc
 
  @Autowired
  lateinit var accountService: AccountService
 
  @Test
  @DisplayName("아무나 접근 가능한 루트 페이지 접근")
  fun index_anonymous() {
    mockMvc.perform(get("/").with(anonymous()))
        .andExpect(status().isOk)
  }
 
 
...
 
}
```

 우선적으로 익명 사용자를 주입해서 아무나 접근 가능한 루트페이지에 정상적으로 OK 이거 나오는지 확인 합니다.

```text
...
  @Test
  @DisplayName("접근 불가능한 페이지 접근시 302 리다이렉트")
  fun success_page() {
    mockMvc.perform(get("/view/success"))
        .andExpect(status().is3xxRedirection)
  }
...
```

인증된 사용자만 접근이 가능한 페이지에 접근시 리다이렉션이 이루어지는지 확인한다.

```text
@Test
@DisplayName("success page 에 인증사용자를 넣어보자. ")
fun user_success_page() {
  mockMvc.perform(get("/view/success").with(user("curtis").roles("USER")))
      .andExpect(status().isOk)
}
```

인증된 사용자를 넣어서 인증된 사용자 가 해당 페이지에 접근 가능한지 확인합니다. 

```text
...
  @Test
  @DisplayName("USER 권한으로 admin 페이지 요청")
  fun user_admin_page() {
    mockMvc.perform(get("/admin").with(user("curtis").roles("USER")))
        .andExpect(status().isForbidden)
  }
...
```

ADMIN 권한만 있는 페이지에 USER 권한이 있는 사용자로 접근을 할경우 forbidden 에러가 나오는지 확인합니다.

```text

....
  @Test
  @DisplayName("ADMIN 권한으로 admin 페이지")
  @WithMockUser(username = "curtisAdmin", roles = ["ADMIN"])
  fun admin_user_admin_page() {
    mockMvc.perform(get("/admin"))
        .andExpect(status().isOk)
  }
....
```

ADMIN 권한이 가진 사용자가 ADMIN 화면에 정상적으로 접근이 가능한지 확인합니다. 

```text

...
  @Test
  @DisplayName(" 어노테이션으로 익명 사용자 주입")
  @WithAnonymousUser
  fun annotation_test() {
    mockMvc.perform(get("/view/success"))
        .andExpect(status().is3xxRedirection)
  }
 
...
```

with\(\) 를 사용하지않고 어노테이션으로 사용자 주입하는 방법입니다. \( 익명사용자 \)

```text

@Test
@Transactional
@DisplayName(" 폼으로 인증 받는 테스트 ")
fun form_user_test() {
  var username = "curtis"
  var password = "1234"
  val account = createAccount(username, password)
  mockMvc.perform(formLogin().user(account.email).password("1234"))
      .andExpect(authenticated())
}
 
private fun createAccount(username: String, password: String): Account {
  val account = Account(null, username, password, mutableSetOf(AccountRole.ADMIN))
  return accountService.saveAccount(account)
}
```

폼 로그인으로 사용되는 폼 부분도 테스트 할수 있습니다  formLogin\(\) 과  user\(\) 를 이용해서 사용. 

![](http://wiki-digit.amorepacific.com:8090/download/attachments/1220862/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-02-07%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%204.36.56.png?version=1&modificationDate=1581061051000&api=v2)

* **이제 위에서 만든 테스트 코드를 구동시 성공하게 됩니다.**
* **테스트 코드를 조금더 다듬을 필요가 있다고 생각...20년 6월 전까지는 코틀린 인 액션을 좀더 정리 해두기..**

\*\*\*\*

* \`@springBootTest\` , \`@autoConfigureMockMvc\` 어노테이선을 달지 않고 , 
* \`@extenWith springExtension.clsass \` 언노테이션이이랑 해당 컨트롤러를 테스트 할수 있습니다.
* \`@WebMvcTest\(AccountController::class\) 로도 가능할것 같아요.
* 참고로 : WebMvcTest 어노테이션은 @Controller , @controllerAdvice 관련된 web 쪽을 사용하게 해주고 , @Service , @compnent , @Repository 는 사용하지 못해서 슬라이스테스트라고 불리우며 통합 테스트보다 좀더 빠르게 테스를 해볼수있어요 \( 필요한 빈들만 주입하기 때문입니다. \) 
* 딱 웹에 집중하게 db에 집중하도록 테스트 할수있는건 @DataJpaTest , @DataJdbcTest 가 있습니다.
* springbootTest 로 통합 테스트 안하고 그냥 웹만 단위로 됬어야했는데..  다음에는 단위로하는 예제를 올려두도록 하겠습니다. 

 감사합니다.

