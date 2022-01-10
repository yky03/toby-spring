
>**AOP**

면접에서 스프링 AOP 가 무엇이냐고 묻는다면?  
->  AOP는 관점 지향 프로그래밍의 약자로써 기존의 OOP에서 기능별로 클래스를 분리했음에도 불구하고, 여전히 트랜잭션, 자원해제, 성능테스트  
메소드처럼 공통적으로 반복되는 중복코드가 발생하는 단점이 생겨서 이를 해결할 수 있도록 개발 코드에서는 비즈니스 로직에 집중하고, 실행 시  
**비즈니스 로직의 앞과 뒤에서 원하는 지점에 해당 공통 관심사를 수행**할 수 있게 하면서 **중복 코드를 줄일 수 있는 방식**이라고 답한다.  

추가적으로 아래의 AOP개념과 용어를 덧붙여 설명하면 Best..  

aop 개념은 IOC/DI 개념과 더불어 스프링에서 아주 중요한 핵심 개념중의 하나이기 때문에,    
스프링 공식 문서를 통해 AOP 정의에 대해 다시 짚어보고,  
개념과 용어에 익숙해지기 위해 머릿속에 그림을 그리면서 다시 정리를 해보자.  

```
Joinpoint: A joinpoint is a candidate point in the Program Execution of the application where an aspect can be plugged in. This point could be a method being called, an exception being thrown, or even a field being modified. These are the points where your aspect’s code can be inserted into the normal flow of your application to add new behavior.

Advice: This is an object which includes API invocations to the system wide concerns representing the action to perform at a joinpoint specified by a point.

Pointcut: A pointcut defines at what joinpoints, the associated Advice should be applied. Advice can be applied at any joinpoint supported by the AOP framework. Of course, you don’t want to apply all of your aspects at all of the possible joinpoints. Pointcuts allow you to specify where you want your advice to be applied. Often you specify these pointcuts using explicit class and method names or through regular expressions that define matching class and method name patterns. Some AOP frameworks allow you to create dynamic pointcuts that determine whether to apply advice based on runtime decisions, such as the value of method parameters.

The following image can help you understand Advice, PointCut, Joinpoints. enter image description here
```
![pointcut image](https://i.stack.imgur.com/J7Hrh.png)  



**-AOP 개념과 용어**
**Aspect**	
: 여러 객체를 관통하는 **‘공통 관심 사항’을 구현**한것을 의미합니다.  
Spring 에서는 설정을 통해 일반 클래스에 넣는 방식(schema-based approach) 혹은 어노테이션을 활용한 방식으로 클래스에 aspect를 줄수 있습니다.  (JointPoint와 PointCut을 묶어서 Aspect로 관리)  

**Join point**  
: **특정 작업이 시작되는 시점**을 나타내는 포인트로, 메서드 호출이나 예외발생 등의 시점들을 의미합니다.  
(어떤 시점(진입전,진입후,예외를 던진 뒤,결과를 던진 뒤 등)에 해야 할 일을 지정)  

**Advice**  
: **특정 join point에서 실행되는 action**을 뜻하며, 실행 시점에따라 ‘around’, ‘before’, ‘after’의 타입들을 가지고 있습니다.  

**Pointcut**	
: **Join point의 부분집합으로써, 실제 Advice가 실행되는 Join point들의 집합을 의미**합니다.  
(JoinPoint가 적용이 되는 대상)  

**Target object**	 
: advise가 적용되어질 타깃 객체를 의미합니다.  

**AOP proxy**  
: Aspect를 구현하기위해 AOP 프레임워크에서 만들어낸 객체를 의미합니다.  

**Introduction**  
: proxy 객체에 메소드나 필드를 추가한것을 의미합니다.  

**weaving**  
: Aspect를 Target object에 적용하는것. 컴파일시, 로드타입시, 런타임시 적용시킬수 있으나 Spring에서는 런타임때 적용시킵니다.  


>**AOP설정**  

강제로 CGLIB 프록시를 사용하려면 요소의 proxy-target-class 속성 값을 true로 설정
```
<aop:config proxy-target-class="true">
```

@AspectJ 자동 프록시 지원을 사용할 때 강제로 CGLIB 프록시를 사용하려면 요소의 'proxy-target-class'속성을 true로 설정
```
<aop:aspectj-autoproxy proxy-target-class="true"/>  
```

spring-boot properties 아래와 같이 설정
```
spring.aop.proxy-target-class=true
```

```
Spring AOP는 JDK 동적 프록시 또는 CGLIB를 사용하여 지정된 대상 객체에 대한 프록시를 만든다.

JDK 동적 프록시 java의 리플렉션을 이용해서 객체를 만든다. CGLIB 경우에는 바이트코드를 조작해 프록시 객체를 만든다.

Spring boot 는 기본적으로 transaction 대상의 aop를 동작시킬 프록시를 cglib 프록시를 사용하게 설정 해놨다.

그리고 성능 또한 jdk 프록시보다는 cglib가 빠르다!!! 우리가 느낄정도? 아니지만 일반적으로 cglib가 예외를 발생시킬 가능성이 낮다고 한다.

프록시 할 대상 객체가 하나 이상의 인터페이스를 구현하는 경우 JDK 동적 프록시가 사용됩니다.타겟 타입에 의해 구현 된 모든 인터페이스는 프록시화된다.

대상 객체가 인터페이스를 구현하지 않으면 CGLIB 프록시가 생성된다.

CGLIB 프록시 (예를 들어, 인터페이스에 의해 구현 된 것뿐만 아니라 대상 클래스에 대해 정의 된 모든 메소드를 프록시하는 경우)를 사용하도록 강제 할 수 있다.
```

*But, spring boot는 인터페이스로 구현해도 아래의 옵션이 디폴트기 때문에 AOP구현시 내부적으로 cglib 방식으로 동작한다.  
spring.aop.auto(default true) 
spring.aop.proxy-target-class(default true)  


<br/>

>**References**  

[스프링 면접 질문](https://kim6394.tistory.com/161)  
[스프링 aop 공식 문서](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#aop)  
[스택오버플로우 joinpoint and pointcut diffrence](https://stackoverflow.com/questions/15447397/spring-aop-whats-the-difference-between-joinpoint-and-pointcut)  
[스프링 aop 원리 jdk dynamic proxy(reflection기반) & cglib proxy(상속기반)]  
[스프링 aop proxy-target-class 속성 설정](https://seungwoo0429.tistory.com/26)  
[스프링 aop CGLIB PROXY 방식 동작 이유](https://multifrontgarden.tistory.com/282)  
[Spring Boot는 왜 CGLIB 방식을 디폴트 전략으로 선택했을까?](http://wonwoo.ml/index.php/post/1708)  
[스프링 aop 개념과 트랜잭션 처리](https://goodncuteman.tistory.com/25)  
[AOP 활용 예제](https://chinggin.tistory.com/516)  
