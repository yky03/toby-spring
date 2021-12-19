
**>예외**

**예외처리** : 프로그램 실행 시 발생할 수 있는 **예외에 대비**하는 것으로 프로그램 비정상종료를 막고 실행 상태를 유지하는 것.  
**에러(error)** : 발생 시 수습할 수 없는 심각한 오류. (컴파일에러, 런타임에러 등)  
**예외(exception)** : 예외 처리를 통해 수습할 수 있는 덜 심각한 오류.  
(예외에는 크게 **Error**(java.lang.Error), **Exception(java.lang.Exception)과 체크예외**, **RuntimeException과 언체크/런타임 예외** 가 있다.)   

<br/>


**>Checked Exception & UnChecked Exception**   








**초난감 예외처리 코드**  

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

**>Tip**
[e.printStackTrace() 를 사용하지 말자](https://tgyun615.com/59)

```
*e.printStackTrace()  

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

**결론은 난감한 예외처리를 하지 말고, 서버에 간단 명료하게 원인을 파악할 수 있는 로그(Slf4j log.error 등)와 함께 상황에 따라 적절한 체크 예외를 던지는것이 좋을 것으로 보인다.  
ex) id가 중복되었으면 DuplicateUserIdException 클래스를 만들어서 RuntimeException 를 상속받아 예외를 넘겨준다.**    

```java
public class DuplicateUserIdException extends RuntimeException {
 puvlic DuplicateUserIdException(Throwable cause) {
  super(cause);
 }
}
```


**추가적으로..**  
[ExceptionHandler 와 ControllerAdvice를 통한 전역 예외 처리](https://tecoble.techcourse.co.kr/post/2021-05-10-controller_advice_exception_handler/)  
[SLF4J- 추상체를 사용해야 하는 이유](https://inyl.github.io/programming/2017/05/05/slf4j.html)  



<br/>



**>References**  

[예외처리](https://itmining.tistory.com/8)  
[throw, throws 차이](https://vitalholic.tistory.com/246)  

