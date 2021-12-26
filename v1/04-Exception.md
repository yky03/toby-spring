
>**예외**

**예외처리** : 프로그램 실행 시 발생할 수 있는 **예외에 대비**하는 것으로 프로그램 비정상종료를 막고 실행 상태를 유지하는 것.  
**에러(error)** : 발생 시 수습할 수 없는 심각한 오류. (컴파일에러, 런타임에러 등)  
**예외(exception)** : 예외 처리를 통해 수습할 수 있는 덜 심각한 오류.  
(예외에는 크게 **Error**(java.lang.Error), **Exception(java.lang.Exception)과 체크예외**, **RuntimeException과 언체크/런타임 예외** 가 있다.)   

<br/>


>**Checked Exception & UnChecked Exception**   

<img src="https://madplay.github.io/img/post/2019-03-02-java-checked-unchecked-exceptions-1.png" width="600" height="500"/>. 


*간단하게 RuntimeException을 상속하지 않는 클래스는 Checked Exception, 반대로 상속한 클래스는 Unchecked Exception.  

<img src="https://madplay.github.io/img/post/2019-03-02-java-checked-unchecked-exceptions-2.png" width="600" height="250"/>.  
  
-예외를 처리하는 방법 3가지
1) 예외 복구 : 예외 상황을 파악하고 문제를 해결해서 정상 상태로 돌려놓는 방법.(catch로 잡아서 retry 등)  
2) 예외 처리 회피 : 예외 처리를 직접 담당하지 않고 호출한 쪽으로 던져 회피하는 방법.(throw e)   
3) 예외 전환 : 예외 회피와 비슷하게 예외를 던지지만, 그냥 던지지 않고 적절한 예외로 전환해서 넘기는 방법.(throw MyCustomException()). 


<br/>

>**초난감 예외처리 코드**  

```java
// 1. 아무 처리도 하지 않는 경우
try {
    ...
} catch (SQLException e) {  //  예외를 잡고 아무것도 하지 않는다. 예외 발생을 무시해버리고 정상적인 상황인 것처럼
                            //  다음 라인으로 넘어가면 안된다.
}

// 2. 단순 출력
} catch (SQLException e) {
    System.out.println(e);
}

// 3. 호출 스택 출력
} catch (SQLException e) {
    e.printStackTrace();
}

// 4. 무책임한 throws
public void method1() throws Exception {
    method2();
    ...
}
public void method2() throws Exception {
    method3();
    ...
}
public void method3() throws Exception {
    ...
}
```

<br/>


>**Tip**

[e.printStackTrace() 를 사용하지 말자](https://tgyun615.com/59)

```
*e.printStackTrace() :  
•예외 발생 당시의 호출스택(Call stack)에 있던 메소드의 정보와 예외 결과를 화면에 출력함  
•예외 상황을 분석하기 위한 용도로 사용 (개발자에게 디버깅 할 수 있는 힌트를 제공)  

사용하지 말아야 하는 이유  
1.printStackTrace()를 call할 경우 System.err로 쓰여져서 제어하기가 힘듬
2.printStackTrace()는 java 리플렉션을 사용하여 추적하는 것이라서 많은 오버헤드가 발생할 수 있음
3.printStackTrace()는 서버에서 스택정보를 취합하기 때문에 서버에 부하가 발생할 수 있음
4.printStackTrace()는 출력이 어디로 가는지 파악하기 가 어려움 (톰캣같은 경우 catalina.out에 남음)
5.printStackTrace()는 관리가 힘듬 (보통 log4j, logback과 같은 로깅 라이브러리를 사용하여, 로그 패턴 및 로그 메세지를 지정 및 콘솔로그 / 파일로그 형태로 관리할 수 있음)

성능을 중시하는 어플리케이션이라면 e.printStackTrace는 사용하지 말자 
```


<br/>



[throw vs throws](https://vitalholic.tistory.com/246)  
-throw : 메소드 내에서 상위 블럭으러 예외를 던지는 것.(프로그래머의 판단에 따른 처리)  
-throws : 현재 메소드에서 상위 메소드로 예외를 던지는 것.(책임전가)  

```java
class Test {
 public static void calculate() throws ArithemeticException {
  int a = 0;
  a = 10/a; // 0으로 나누면 예외 발생.
 }
 public static void main(String[] args) {
  try {
   Test.calculate(); // 예외를 던짐.
  } catch (Exception e) {
    System.out.println("메인 메소드가 예외를 잡아서 처리함: " +e);
    throw new MyException();
  }
}
```

<br/>  

>**결론**

: 난감한 예외처리를 하지 말고, 서버에 간단 명료하게 원인을 파악할 수 있는 로그(Slf4j log.error 등)와 함께 상황에 따라 적절한 체크 예외를 던지는것이 좋을 것으로 보인다.  
ex) id가 중복되었으면 DuplicateUserIdException 클래스를 만들어서 RuntimeException 를 상속받아 예외를 넘겨준다.  

다시 정리하면 자바에서 예외는 **RuntimeException을 상속하지 않고 꼭 처리해야 하는 Checked Exception** 과 반대로 명시적으로 처리하지 않아도 되는 **Unchecked Exception** 으로 구분할 수 있다.  


```java
public class DuplicateUserIdException extends RuntimeException {
 puvlic DuplicateUserIdException(Throwable cause) {
  super(cause);
 }
}
```

<br/>


>**추가적으로..**  

[ExceptionHandler 와 ControllerAdvice를 통한 전역 예외 처리](https://tecoble.techcourse.co.kr/post/2021-05-10-controller_advice_exception_handler/)
[ExceptionHandler 단일, ControllerAdvice 전역](https://sunghs.tistory.com/121)  
[SLF4J - 추상체를 사용해야 하는 이유](https://inyl.github.io/programming/2017/05/05/slf4j.html)  
[SLF4J - appender](https://enai.tistory.com/36)  




<br/>


>**Q&A**

**-예외 Best Practice 8가지**    
1) THROWABLE(모든 예외의 상위 클래스 - 에러까지 잡는다)예외 처리로 하지 마라.  
2) 입력을 빨리 검증하라 (예외처리 최소화)  
3) 예외로 흐름을 제어하지 마라(예외 에서 분기)   
4) 하나의 예외로그는 한줄의 예외로그로 제어(키값으로 제어 가능 GREP)    
5) 예외를 감싸서 던져라(예외전파(프로파제이션),예외체이닝,예외래핑)  
6) FINALLY 블록에서 예외 던지지 마라 ? 이전까지 스택트레이스 기록 불가능  
7) 예외로그를 남기고 똑같은 예외를 한번 더 던지지마라  
8) 빨리 던지고(예외는 최대한 빨리) 늦게 잡아라(적절한 위치(최상위 최하위 레이어 등)가 아닌곳에서 무리한 예외처리 할필요 x)  

<br/>

**-Checked Exception 에서 롤백처리가 되지 않는가에 대해서(중요)**  

구글링을 하면 Checked Exception 은 롤백이 되지 않는다고 정리된 표들이 많은데,    
이는 특정 출판사에서 출판한 책 내용에서 잘못된(중요 정보 생략)정보로 부터 계속 파생되어 공유되고 있다고 본 것 같습니다.   
스프링에서는 설정을 변경하면 디폴트 롤백 설정을 변경 할 수 있다고 합니다.   

관련 블로그 내용에서 아래의 내용을 우선 확인하였습니다.  
( https://www.notion.so/3565a9689f714638af34125cbb8abbe8 ​)  

  
-> java와 spring 개념이 혼용되며 혼란을 야기함.  
-> spring의 기본적인 트랜잭션 설정은 checked 는 롤백하지 않고, unchecked는 롤백한다.  
   ​- but. 기본적인 설정일 뿐 변경할 수 있다.  

  
추가적으로 설정을 변경하여 checked exception 에서 설정을 변경하여 롤백 처리를 한 블로그들입니다.  
http://wonwoo.ml/index.php/post/1542  
https://techblog.woowahan.com/2606/  

<br/>

**-실패 원자성을 갖도록 노력하자**  
https://brunch.co.kr/@oemilk/178  
[이펙티브자바 - 가능한한 실패 원자적으로 만들라](https://madplay.github.io/post/strive-for-failure-atomicity)  

**-멀티 쓰레드 환경에서 동시성 제어**  
https://deveric.tistory.com/104

<br/><br/>

>**References**  

[예외처리](https://itmining.tistory.com/8)  
[throw, throws 차이](https://vitalholic.tistory.com/246)  
[Checked Excepton, Unchecked Exception](https://madplay.github.io/post/java-checked-unchecked-exceptions)   
[예외처리꿀팁](https://velog.io/@sangmin7648/Java-%EC%98%88%EC%99%B8-%EC%B2%98%EB%A6%AC-%EA%BF%80%ED%8C%81)   


