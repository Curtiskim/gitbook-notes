---
description: kotlin in action 책을 공부하며 정리해보는 내용입니다.
---

# Kotlin in Action 책 정리노트

{% hint style="info" %}
들어가기 앞서 아래 내용은 일반적으로 구글링을 하거나 책을 보면 다 나오는 내용입니다. 정리를 한번 해볼까 생각이 들었던 이유는 요듬 대세인 코틀린을 1도 몰랐던 상황에서 조금 벗어나고 싶었고, confluence 에 블로깅 기능이 있길래.. 그냥 한번씩 틈날때마다 정리를 해보려고 합니다. \( 공부겸 \) 또한가지 2019년 11월 28일 있었던 NHN 컨퍼런스에 참가하게 되어 여러 세션을 듣던 도중 3~4개 세션이.. 코틀린 내용이 있었습니다. NHN 에서 java 2 kotlin 으로 변경하는 많은 프로젝트를 보며 우리도\(AP\) 할수 있겠다라는 생각이 들었으며, \( 실제 NHN에서 근무하던 같은 팀이였던 여러 동료들이 해당 세션을 발표하고 소개해주고 저에게도 귀에 피가 나도록 kotlin을 추천했습니다.....\) 이미 많은 선배 개발자 및 후배 개발자 분들이 kotlin 을 사용하며, java 에서 kotlin 으로 변경하는 일을 하는 모습을 보고, 나도 한번 다음 프로젝트는 kotlin 으로 해야겠다는 다짐을 하며 포스팅을 시작합니다. \( 사실 이 포스팅이 계속 이어질지는 의문이긴 합니다만... 그래도 한번 해보죠.. \)
{% endhint %}

##  탭 순서대로 정리를 하겠습니다. 

{% tabs %}
{% tab title="kotlin 1" %}
## 코틀린 맛보기

```text
data class Person(
        val name: String,
        val age: Int? = null
)

fun main(args: Array<String>) {

    val persons = listOf(Person("kim"), Person("curtis", age = 34))
    println(persons)
    val oldest = persons.maxBy { it.age ?: 0 }
    println("나이가 가장 많은 사람은 : $oldest")

}
```

**위 코드를 작성시 프린트 되는 내용**

```text
[Person(name=kim, age=null), Person(name=curtis, age=34)]
나이가 가장 많은 사람은 : Person(name=curtis, age=34)
```

## kotlin 특징

1. **코틀린 주요 특징**
   1. 대상 플랫폼 : 서버 , 안드로이드 ,등  자바 실행되는곳.
   2. 타입 지정언어가 아니다. 
      1. 정적 타입언어는 \( 성능 , 신뢰 , 유지 보수 ,도구 지원 \) 이런 장점이 있습니다.  → 코틀린은 타입 추론을 지원하는 정적타입 지정 언어!!
         1. 하지만 코틀린은 직접 안해도 된다.  \( ex  var x = 1  , var y ="curtiskim"  \)  → 코틀린은 컴파일러가 타입을 자동으로 추론 해준다.  그래서 타입을 지정 할 필요없음\( 타입추론 \)  , 참고  :  java 11인지 12 부터  java 도 var 변수에 담을 수 있다. 생각보다 유용하다.
   3. 함수형 ,객체지향 , 
      1. java는 객체지향 언어라고 한다.  → 코틀린은 함수지향이며 객체지향적으로 가능하다.  \( 물론 제 생각은 java 도 객체지향이지만 함수형으로도 가능하다고 생각한다. \) 
      2. 함수형은 함수를 일반값처럼 다룰수 있음. 
      3. 불변값 지원 으로 멀티 스레드에서 개발 및 테스트가 쉽다.
   4. 무료 오픈소스 
   5. IDE에 완벽하다고 생각한다.
   6. 서버 애플리케이션에도 ,  html 생성 , 영속화 등 일반적인 작업이 다 가능 
   7. 안드로이드에서도 사용가능.
   8. 등등 많은 
2. **코틀린 철학**
   1.  실용성 
      1.  IDE 지원이 중요한 요즘 아주 적합.
   2. 간결성
      1. 코드를 새로 작성하는 시간이 기존 java 보다 절약됨 
      2. get ,set  construcotor 이런걸로 지저분해지지 않고 번거로운 준비를 안해도됨  ... \(물론 java lombok 이런거쓰면 그래도 편한데. kotlin은 더 간결하고 읽기 좋아지는 것 같다. \) 

```text
// kotlin
data class Person(
        val name: String,
        val age: Int? = null
)
// getter , setter , toString ... 등 많은것들을 내포 


// java 
/**
 * @author bongjun.kim
 */
public class Account {
  private String name;
  private Integer age;

  public Account(String name, Integer age) {
    this.name = name;
    this.age = age;
  }

  public String getName() {
    return name;
  }

  public void setName(String name) {
    this.name = name;
  }
...
...
```

### 안정성

1. 프로그래밍 오류중에 일부 유형의 오류를 설계에서 막아준다. \( 이게 우리가 가장잘 알려진 코틀린 특징중 널 세이프티 하다 라는 말을 많이 들었을 것 이다. \)
   1. 그래서 안전한 널이 어떤건지 한번 알아보자.

```text
val s: String? = null // null 가능 > ? 덕분
val s2: String =""  // null 불가
```

우선 위 코드만 보면 그래서 어떤게 널 세이프티야 ? 라고 생각이 들수도 있습니다. \( 한번에 이해 하셨을수도 있습니다. \) 그런데 아래 예제를 한번더 보면 감이 올수 있습니다.

```text

// java 
System.out.println("여기에 비즈니스1 로직 이 있다고 가정");
Integer a = null;
int b = a;
System.out.println("여기에 비즈니스2 로직 이 있다고 가정");

// 비즈니스로직 1이 수행되고 런 타임에 NPE 가 발생. 

// kotlin

println("여기에 비즈니스1 로직 이 있다고 가정")
val a: Int
val b = a  // 여기서 컴파일 에러가 발생되어 실행 조차 되지 않는다.
println("여기에 비즈니스2 로직 이 있다고 가정")

// Variable 'a' must be initialized  컴파일 에러
```

1. 1. 위 상황에서 볼수 있듯이 애플리케이션에서 발생할 수 있는 NPE 같은 중단을 많이 줄일수 있게 되었다 \( kotlin 을 사용하는것 만으로도 NPE 걱정이 주는샘 \) 
2.  ClassCastException 을 방지 해준다.
   1. 어떤 객체의 타입을 확인하고,  그 타입이 같은 타입이면 변경해준다고할때.

```text

var value = "Hello";
if (value is String) // type을 확인.
    println(value.toUpperCase()) // String 의 toUpperCase 를 사용한다.
```

### 상호 **운용성**

1.  기존 java 로 되어 있는 프로젝트를 보면 기존에 라이브러리는 분명 java 로 되어있을 것이다 . 우리는 많은 라이브러리들을  그래들 디팬더시를 이용하여 받아서 사용하고있다. 
   1. 코틀린의 경우 java 에서 사용하던 라이브러리 그대로 가져다 사용할수있다!!! \( 아주 칭찬한다. \) 
   2. 또한  코틀린에서 자바 메소드를 호출하거나 , 상속 , 인터페이스 등 코틀린에 적용하는 일이 모두 가능하다
2.  위와같은 특징때문에 기존 java 프로젝트를 kotlin 으로 변경하는건 생각보다 간단하게 변경할 수 있다.  \( 하지만.. 실전은 생각보다 고통스러울 수도 있다.\)
   1. 변경 도전해볼만한 이유 :  intelij 에 코틀린 플러그인을 받은 이후 커맨드+옵션+쉬프트+K 를 누를경우 java로 변경된걸 자동 컨버팅 해준다.   \( java 소스를 그냥 카피 하고 .kt 파일에 붙여넣기 해줘도 동일변경 \)
{% endtab %}

{% tab title="kotlin 2" %}
### 코틀린 기본 문법 기초

## 함

```text
/
fun max(a: Int, b: Int): Int {
    return if (a > b) a else b
}
```

코틀린 함수 선언은 위 예제 코드에서 보는거와 같이 fun 키워드를 사용한다 .

{% hint style="info" %}
fun 선언,   max 함수명  ,   \(a: Int, b: Int\) 파라미터 타입 , :Int 리턴 타입 
{% endhint %}

위에서 선언한것을 코틀린은 조금더 간결하게 만들어 줄수 있습니다. 예를들면 리턴타입을 생략할 수 있습니다.

```text
fun max(a: Int, b: Int) = if (a > b) a else b
```

이렇게 리턴타입을 생략 할수있는것은 코틀린의 특징인 타입 추론 덕분 입니다.

## 변수

코틀린은 크게는 val , var 변수를 선언 , val 은 java에서의 final 키워드가 있는 변수라고 생각하면 됩니다.

```text
val a = "curtiskim" // immutable
var b = "curtis" // mutable
```

또한 코틀린은 타입추론이 가능하여 타입을 생략해서 var , val 로 변수를 지정해주면 자동으로 타입지정됩니다. \( 하지만 초기화를 하지 않은 상태에서는 반드시 리턴타입을 지정을 해줘야합니다.

```text
var a = "안녕하세요"  // 스트링
var a = 10  // int 
var a = 6.222 // double 


var a : int  // 초기화 하기 전에 리턴타입 지정
a = 35
```

코틀린 개발 가이드에서는  기본적으로 변수는 불변 val 로 선언해서 사용하고 , 나중에 필요한경우 var 로 변경해서 사용하라고 합니다. 

불변 객체는 한번만 초기화가 되어야 하는데 또다른 특징으로는  컴파일러가 확인할수만 있다면 조건문 에 따라 다른값으로 초기화 할수있습니다.

```text
fun test() { 
    val message: String
    if(max(1,2) > 10) {
        message = "success"
        // 연산
    } else {
        message = "fail"
    }
}
```

코틀린은 변수의 타입을 지정하지 않아도 초기화를 할수있지만 한번 초기화된 변수의 _타입은 변경되지 않는다_

```text
var a = 320
a = "curtiskim"   // 컴파일 오류
```

위 처럼 코틀린 **타입추론**은 초기화 시점에 이루어 집니다. 

문자열을 탬플릿 처럼 사용할수있는 기능을 제공 합니다.   \( 변수를 사용할때 $ 를 이용해서 사용할수 있습니다. \) 참고 : 컴파일된 코드는 내부적으로 StringBuilder 를 사용합니다.  append로 문자열 빌더 뒤에 추가합니다. 

```text

fun main() {
    val text = "world"
    println("hello curtis $text!!")
}


// 출력결과  hello curtis world!!
```

위에서 나아가서 **중괄호를** 이용해서 더 많은 일을 할 수 있습니다.

```text

fun main(args: Array<String>) {
    val text = "world"
    println("hello curtis $text!!")

    println("hello , ${if (args.size > 0) "true" else "false"}  world !!! ")
    println("hello , ${if (args.size > 1) "true"  else "여기서도 쓸 수 있어요 $text "}  curtis !!! ")
}


// 출력 : 
// hello curtis world!!
// hello , false  world !!! 
// hello , 여기서도 쓸 수 있어요 world   curtis !!!
```
{% endtab %}

{% tab title="kotlin 3" %}
### 코틀린 클래스 생성 기본 문법 <a id="kotlin3-&#xCF54;&#xD2C0;&#xB9B0;&#xD074;&#xB798;&#xC2A4;&#xC0DD;&#xC131;&#xAE30;&#xBCF8;&#xBB38;&#xBC95;"></a>

```text
// java
public class Bong {
  private String title;
 
  public Bong(String Title) {
    this.title = title;
  }
 
  public String getTitle() {
    return title;
  }
}
 
 
// kotlin
class Bong(val title: String)

```

위 처럼 클래스를 만드는 것도 java 보다 꽤 많이 간결해집니다.

아래 내용은 생성자  및 get 사용법 입니다.  **\(사용법이 간결해 질수 있습니다.\)** 

```text
val bong = Bong("hello World")  // new 키워드를 사용하지 않고 생성자 사용
val getBongTitle = bong.title // 프로퍼티 이름을 그대로 사용하면 get 자동으로 호출
```

### 프로퍼티

클래스라는 개념의 목적은 데이터를 캡슐화 하고 데이터를 다루는 코드를  가두는데에 목적이 있다고 합니다.  대부분 \( private 비공개 로 필드를 선언하고 ,set, get ,add .... \)으로  제공 하죠 

java 에서 필드를 접근자 메소드 레벨을 정할수있습니다 \( private ,public , protected , default \) 

접근자 + 필드 = 프로퍼티라고 합니다 \(  public String name; \) 

{% hint style="success" %}
  
val : 읽기 전용 // final 비공개 필드와 공개 get 제공

var : 변경 가능 // 쓸수있는 프로 퍼티로 비공개 필드 , get ,set 을 제공
{% endhint %}

### 커스텀 접근자

```text
class Rectangle(val height: Int, val width: Int) {
  val isSquare: Boolean
    get() {     // get 선언.
      return height == width
    }
}

```

여기서 isSquare 프로퍼티는 자체 값을 저장하는 필드가 필요없고 get 만 존재한다. 사용은 아래 와 같이

```text
val rectangle = Rectangle(10 ,10)
println(rectangle.isSquare)
>>> true
```

### 디렉토리와 패키지

```text
package com.example.kotlindemo.domain  // 패키지 선언.
 
import Account   // <--- java 클레스도 임포트 가능.
```

java 와 동일하게 위 처럼 사용 가능하다.   

java 랑 동일하게도 패키지 뒤에 \* 를 선언하면 임포트 할수있다.    \(   import com.bong.\*    \) 

java 에서는 패키지 구조와 동일한 디렉토리 계층구조를 만들고 , 위치패야했고 또한 클래스명과 파일명이 동일했어야 했지만 코틀린에서는 그게 무시됩니다.

{% hint style="danger" %}
코틀린은 디스크상 어느 디렉토리 안에서나 넣을수있으며 하나의 클래스 파일에  여러개의 클래스도 만들수있으며 , 심지어 파일이름도 마음데로 정할수있습니다. \( 조금 변태같음 \)

하지만 실제로 사용할때는 자바와 동일하게 사용하는게 가독성도 좋으며  ,나중에 java 와 함께 쓰기에도 좋다고 합니다.  

하지만 가이드에서는 클래스를 한파일에 몰아 넣는것을 주저 하지 말라고 합니다.   \(클래스 코드가 작다면 \) 
{% endhint %}

### 선택 표현 처리 enum , when  <a id="kotlin3-&#xC120;&#xD0DD;&#xD45C;&#xD604;&#xCC98;&#xB9AC;enum,when"></a>

#### enum

```text
enum class Color {
  RED, ORANGE, YELLOW, GREEN, BLUE, INDIGO, VIOLET
}
```

여기서 차이점은 java 는 enum , kotlin 은 enum class java 에서 와 마찬가지로 코틀린도 이넘에 프로퍼티 를 만들고 메소드를 만들 수 있습니다.

```text
enum class Color(
    val r: Int,
    val g: Int,
    val b: Int
) {
  RED(255, 0, 0),
  ORANGE(255, 164, 9),
  YELLOW(255, 255, 0);
 
  fun rgb() = (r * 256 + g) * 256 + b
 
}
 
println(Color.RED.rgb())
```

#### when <a id="kotlin3-when&#xC0AC;&#xC6A9;"></a>

java 에서 switch 와 비슷하다고 볼 수 있습니다.

```text
fun getEnumWhenTest(color: Color) = when (color) {
  Color.ORANGE -> "주황"
  Color.RED -> "빨강"
  Color.YELLOW -> "노랑"
}
 
 
println(getEnumWhenTest(Color.RED))
```

자바도 가능해요 사실...

자바 12부터 코틀린에서 사용하는 when 처럼 향상된 switch 문을 사용할 수 있습니다. 

java 에서 break 를 넣지 않아도 되며 java 에서 빼먹으면 안된다고 하는 java 12 부터 생긴 좋아진 스위치문은 위와 비슷합니다.  \( 아래 참조 \) 

```text
// java 에서 사용하는것도 아래처럼 쓸수있음 12에서도 preview 로 해야 쓸수 있습니다  ,또한 var 로 변수 선언도 가능합니다~ ( 11부터 였던거 같은데 정확하게 기억은 안나유. )
 
int numLetters = switch (day) {
    case MONDAY, FRIDAY, SUNDAY -> 6;
    case TUESDAY                -> 7;
    case THURSDAY, SATURDAY     -> 8;
    case WEDNESDAY              -> 9;
};
```

위에 자바에서 , 로 구분지어 사용한것도 코틀린에서도 가능 합니다. 

또한 import 를 해버리면 Color.RED 처럼 사용하지 않고  RED 를 그냥 이름만으로 사용할 수 있습니다. \( 스태틱 임포트와 같다고 보시면 됩니다. \) 

그리고 java 스위치 문보다 좋다고 말하는 부분을 조건을 임의의 객체를 허용할수 있다고 합니다.

아래 예제를 보면 두개의 색을 받아서 혼합해서 하나의 색을 내주는 함수를 만들어볼수 있습니다.

```text
fun mix(c1: Color, c2: Color) = when (setOf(c1, c2)) {
  setOf(RED, ORANGE) -> ORANGE
  setOf(ORANGE, YELLOW) -> RED
  else -> throw Exception("Dirty color")
}
 
 
println(mix(RED,YELLOW))   // Dirty color
```

여기서 나아가 인자 없는 when 절도 만들수 있습니다.

```text

fun mixOptimized(c1: Color , c2: Color) = when {  // 여기 when(color) 인자를 없이 쓸수있음 +_+
    (c1== RED && c2==YELLOW) || (c1== YELLOW && c2==RED) -> ORANGE
    (c1== ORANGE && c2==YELLOW) || (c1== YELLOW && c2==ORANGE) -> RED
    else -> throw Exception("Dirty color")
}
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
탭으로 나눠서 코틀린을 정리를 조금해봤는데요.. when 까지바께 못정리를 했네요.. 

여기에 나오는 내용들은 kotlin in action 책을 기반으로 작성을 했으며 구글링을 통해서 정리했습니다. 

책을 마져 읽고 조금더 정리해보도록 할께요. 
{% endhint %}

