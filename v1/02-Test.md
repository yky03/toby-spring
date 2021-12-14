
# 02. 테스트

> **테스트란**  
  
: **스프링**이 개발자에게 제공하는 가장 중요한 **가치**가 무엇이냐고 질문한다면 주저하지 않고 **객체지향과 테스트**라고 대답할 수 있다.(**그만큼 중요하다는 뜻**)
-> 스프링으로 개발을 하면서 테스트를 만들지 않는다면 이는 스프링이 지닌 가치의 절반을 포기하는 것과 마찬가지이다.  

<br/>

> **유용성(사용성)**   

원초적으로 테스트 코드를 작성한다고 하면 우리는 메인 메소드를 만들고,  
applicationContext 객체를 만들어서 출력값을 찍어서 눈으로 직접 판단을 할 것이다.   

또는 웹 URI를 통해 직접 접근하여 JSP, CONTROLLER, SERVICE 까지 샘플 코드를 전부 만들어야 코드에 대한 테스트가 가능하다.  
이러한 방법은 내 기능에 대한 단위별 테스트가 정확히 되지 않고 환경에 따라 테스트 결과가 달라질 수 있으며 생산성도 느려진다.  

따라서 테스트는 **작은 코드로 단위 테스트(unit test)** 하여 테스트 하도록 해야 한다.[(폭포수 모델 -> 빠른 피드백의 애자일 모델)](https://universitytomorrow.com/19)  

<br/>
![폭포수 애자일 비교](https://myservername.com/img/other/98/agile-vs-waterfall-which-is-best-methodology-2.png)  
<br/>

수동 확인 작업의 번거로움(단지 콘솔에 출력된 값으로 판단하는건 사람의 몫이기 때문에 위험)을 피하기 위해 [단정문(Assert)](http://junit.sourceforge.net/javadoc/org/junit/Assert.html)을 사용하는것이 좋다.   

<br/>

> **JUnit이란**   

: **자바의 단위 테스팅 도구**이며 단 하나의 jar파일로 되어있다.  
테스트 클래스를 별도로 만들어서 테스트 케이스와 히스토리를 남기며 어노테이션과 단정문으로 테스트 케이스를 실행시켜 실패, 성공 등의 코드에 대한 테스트를 할 수 있게 한다.  

<br/>

> **단정문이란**  

말 그대로 **결과를 단정** 짓는 것이다.  
<br/>
접두사에 **assert**가 붙는것이 주 특징이며 예상값과 결과값을 비교해 프로그램(사람의 눈이 아닌)이 결과물을 테스팅 할 수 있도록 한다.    

```
· assertTrue([message], expected) : expected 값이 참이어야 함! 아니면 실패(fail)
   - [message] : assert문을 실행할때 나오는 메시지. 생략가능하지만 써주는것이 정말 좋은 습관임!!
      expected  : 예상하는 기대값.  

· assertEquals([message], expected, actual) : 기대값(expected)이 실제로 나오는 값(actual)과 같아야 함! 아니면 실패(fail)
  - [message] : assert문을 실행할 때 나오는 메시지.
      expected : 예상하는 기대값.
      actual       : 실제로 실행해서 나오는 값.

이외 assertFalse, assertSame, assertNotSame, assertNull, assertNotNull 등과

Hamcrest 의 assertThat이 있습니다.
```

```java
// 대표적인 단정문(assertXXXX)
assertArrayEquals(a, b) : 배열 a와 b가 일치함을 확인
assertEquasl(a, b) : 객체 a와 b의 값이 같은지 확인
assertSame(a, b) : 객체 a와 b가 같은 객체임을 확인
assertTrue(a) : a가 참인지 확인
assertNotNull(a) : a객체가 null이 아님을 확인
```

<br/>

> **assertTrue보다는 assertThat 등 Hamcrest을 사용하자**   
 
-Failure 메시지의 가독성  
-테스트 코드의 가독성  
-다양한 Matcher 제공  

```
먼저 assertTrue를 사용하는 예와 실패 메시지를 살펴보자.
assertTrue(expected.contains(actual));

--- 실행 결과 ---
java.lang.AssertionError
	at org.junit.Assert.fail(Assert.java:86)
	at org.junit.Assert.assertTrue(Assert.java:41)
	at org.junit.Assert.assertTrue(Assert.java:52)
특정 string을 포함하는지 확인하기 위해서는 assertStringContains와 같은 메서드가 없기 때문에 contains 메서드를 사용하고 assertTrue을 이용해 검증해야 한다. 여기서 문제는 assertion error는 expected 값과 actual 값에 대해 알려주지 않는다는 것이다.

->->->->->->->->->->->->->->->->->->->->->->->->->->->->->->->->

assertThat을 사용하도록 변경해보자.
assertThat(actual, containsString(expected));

--- 실행 결과 ---
Expected: a string containing "expected"
     but: was "actual"
java.lang.AssertionError: 
Expected: a string containing "expected"
     but: was "actual"
	at org.hamcrest.MatcherAssert.assertThat(MatcherAssert.java:20)
expected 값과 actual 값 모두 에러 메시지에 반환된다. 원인을 찾기 위해 별도의 디버깅 필요없이 에러 메시지 만으로 잘못된 부분을 바로 파악할 수 있다.
```

<br/><br/>


> **예외조건 테스트(익셉션 뿐만 아니라 예외 케이스 등 테스트를 작성할 때 부정적인 케이스를 먼저 만드는 습관을 들이는 게 좋다.)**  

```java
@Test(expected=XXXXExcepion.class)  
```

<br/><br/>


> **테스트(코드) 커버리지**  

**코드 커버리지란 소프트웨어 테스트가 '잘', '성공적으로' 수행됬는지 판단하는 기준으로 활용**할 수 있다.  
크게 **구문, 조건, 결정의 요소**로써 각 요소를 얼마나 수행했냐에 따라 **비율**을 나타낸다.  

예를 들어, 가장 많이 체크되는 구문 요소중 if else 문이 6개 라인 대상 중 else문을 제외한 5 개 라인만 테스팅 되었다면   
계산식은 5개/6개 * 100 = **83.33%** 가 된다.  

```
소프트웨어 공학적으로 코드 커버리지는 어떤 기준으로 측정할까?  
코드 커버리지는 소스 코드를 기반으로 수행하는 **화이트 박스 테스트**를 통해 측정한다.
(즉, 내부 구조와 동작을 아는 상태에서 테스트)

*화이트 박스 테스트(White-box test)
- 응용 프로그램의 내부 구조와 동작을 검사하는 테스트 방식이다.
- 소프트웨어 내부 소스 코드를 테스트하는 기법이다.
- 개발자 관점의 단위 테스트 방법이라 볼 수 있다.

*블랙 박스 테스트(Black-box test)
- 소프트웨어의 내부 구조나 작동 원리를 모르는 상태에서 동작을 검사하는 방식이다.
- 올바른 입력과 올바르지 않은 입력을 입력하여 올바른 출력이 나오는지 테스트하는 기법이다.
- 사용자 관점의 테스트 방법이라 볼 수 있다.

```

<br/>

> **TDD 3가지**   

: **TDD란 Test Driven Development의 약자로써 말그대로 테스트를 주도하며 개발을 진행**하는 것이다.  
<br/>
크게 3가지의 원칙을 기억하면 되는데,   

<br/>

**1) 질문** : 테스트 작성을 통해 시스템에 질문. ( 테스트 실패 )  
**2) 응답** : 테스트를 통과하는 코드를 작성해서 질문에 대답. ( 테스트 성공 )  
**3) 정체** : 통합, 불필요한 부분 및 중복 제거 등의 리팩토링.  

**반복** : 위 3가지를 반복을 통해 계속 작성한다.  

<br/><br/>

> **테스트케이스는 동등분할이나 경계값 분석을 적극적으로 테스트하자**  

**-동등분할** :  
같은 결과를 내는 값의 범위를 구분해서 **각 대표값으로 테스트** 하는 방법.  
(true, false, 예외 3가지 케이스라면 각각의 입력값이나 상황을 찾아 모든 경우를 테스트)    

**-경계값분석** :  
에러는 동등분할 범위의 경계에서 주로 많이 발생한다는 특징을 이용해서 **경계의 근처에 있는 값을 이용해 테스트** 하는 방법.    

<br/><br/>

> **2장 테스트 정리**   

-테스트는 **자동화** 되야 하고, **빠르게 실행**할 수 있어야 한다.   
-main() 테스트 대신 **Junit 프레임워크를 이용**한 테스트 작성이 편리하다.   
-테스트 결과는 **일관성**이 있어야 한다. 코드의 변경 없이 환경이나 테스트 실행 순서에 따라서 결과가 달라지면 안 된다.  
-테스트는 포괄적으로 작성해야 한다. 충분한 검증을 하지 않는 테스트는 없는 것보다 나쁠 수 있다.  
-코드 작성과 테스트 수행의 간격이 짧을수록 효율적이다.  
-**테스트하기 쉬운 코드가 좋은 코드**다.  
-테스트를 먼저 만들고 테스트를 성공시키는 코드를 만들어가는 **테스트 주도 개발 방법**도 유용하다.  
-테스트 코드도 애플리케이션 코드와 마찬가지로 적절한 **리팩토링** 필요하다.  
-@Befire, @After를 사용해서 테스트 메소드드르이 공통 준비 작업과 정리 작업을 처리할 수 있다.  
-스프링 테스트 컨텍스트 프레임워크를 이용하면 테스트 성능을 향상시킬 수 있다.  
-동일한 설정 파일을 사용하는 테스트는 하나의 애플리케이션 컨텍스트를 공유한다.  
-@Autowired를 사용하면 컨텍스트의 빈을 오브젝트에 DI 할 수 있다.  
-기술의 사용 방법을 익히고 이해를 돕기 위해 학습 테스트를 작성하자.  
-오류가 발견 될 경우 그에 대한 **버그 테스트를 만들어두면 유용**하다.   

<br/>

**스프링을 사용하는 개발자라면 자신이 만든 코드를 테스트로 검정하는 방법을 알고 있어야하며,   
테스트를 개발에 적극적으로 활용할 수 있어야 한다.**  

<br/><br/>

> **추가적으로..  Reference참고** 

-[Junit4와 JUnit5에 대해서](https://blog.naver.com/ykycome00/222271373416)  
: junit5를 사용하면 public 생략 가능, displayname 등 신규 어노테이션도 사용 가능하다.  
<br/>
<img src="https://goodgid.github.io/assets/img/junit/Junit5-Intro-Structure_1.png" width="500" height="500"/>

```
-JUnit Platform
JUnit으로 만든 Test Code를 실행시키는 Launcher를 의미한다.
Launcher를 통해서 콘솔에서도 테스트 실행이 가능하다.
TestEngine API를 제공한다.

-JUnit Vintage
JUnit3과 4의 구현체이다.
= TestEngine API 구현체이다.

-JUnit Jupiter
JUnit5 구현체이다.
= TestEngine API 구현체이다.
Spring Boot에서는 2.2 올리면서 Default로 설정되었다.
그래서 프로젝트를 생성하면 기본적으로
Dependency에서 vintage-engine은 exclude가 되어있다.
```


<br/>

>-[Mockito](https://velog.io/@max9106/Mockito-Mockito%EB%9E%80)    

: **Mock(진짜 객체 처럼 동작하지만 프로그래머가 직접 컨트롤 할 수 있는 객체)을 지원하는 프레임워크.**     
가짜 mock 객체를 만들어 예외를 던지거나 특정 결과값에 대한 조건을 지정하여 테스트가 가능하다.  
[mockito사용법 - 이해가 쉬운 예제 정리](https://jdm.kr/blog/222)   

<br/>

>Tip

: [SpringFramework 에도 실제 사용중인 junit 코드가 있다.](https://github.com/spring-projects/spring-framework/blob/main/spring-core/src/test/java/org/springframework/util)   


<br/>

>**회고 한줄평**   

코드를 먼저 구현하고 테스트 코드를 주로 만들고 있는데,  
TDD의 방향성과 비슷하게 **테스트 코드를 중간중간 먼저 만들면서(실패케이스와 단조로운 성공 케이스 and 메소드화) 개발을 진행**해야 할 것 같다. 
(But, 무조건 TDD대로만 할 필요는 없고 **주어진 일정과 개발 환경을 고려하여 최선의 방법을 찾아 적용하고 개선하는 것이 중요**할 것 같다.)  
또한 테스트코드를 많이 작성할수록 개발한 **코드에 대한 검증을 통해 자신값을 갖고 코드 리팩토링을 자유롭게** 할 수 있다.(결과적으로는 생산성 향상)   
<br/>
추천 책 : [켄트백의 테스트 주도 개발](https://aalphaca.tistory.com/41)   

<br/>

>**-References**     

[Junit의 동작 방식](https://goodgid.github.io/How-JUnit-Works/)   
[Junit5 구조](https://goodgid.github.io/Junit5-Intro-Structure/)  
[Assert단정문](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=k_builder&logNo=40192031458)  
[단정문 및 어노테이션 정리](https://ejyoo.tistory.com/236)  
[Hamcrest - assertTrue 보다는 assertThat을 사용하자](https://jongmin92.github.io/2020/03/31/Java/use-assertthat/)  
[Junit에서 Hamcrest를 사용하는 이유](https://codechacha.com/ko/how-to-use-hamcrest-in-junit/)  
[코드커버리지](https://tecoble.techcourse.co.kr/post/2020-10-24-code-coverage/)  
[TDD](https://lee1535.tistory.com/108)    
[토비의 스프링 2장 총정리](https://junghyungil.tistory.com/154)  
