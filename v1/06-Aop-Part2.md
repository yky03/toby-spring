
**>AOP**

스프링 공식 문서를 통해 AOP 정의에 대해 다시 짚어보자.  


**>AOP설정**  

```
강제로 CGLIB 프록시를 사용하려면 요소의 proxy-target-class 속성 값을 true로 설정
<aop:config proxy-target-class="true">
```
```
@AspectJ 자동 프록시 지원을 사용할 때 강제로 CGLIB 프록시를 사용하려면 요소의 'proxy-target-class'속성을 true로 설정
<aop:aspectj-autoproxy proxy-target-class="true"/>  
```
```
spring-boot properties 아래와 같이 설정
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


**>References**  

[스프링 aop 공식 문서](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#aop)  
[스프링 aop 원리 jdk dynamic proxy(reflection기반) & cglib proxy(상속기반)]  
[스프링 aop proxy-target-class 속성 설정](https://seungwoo0429.tistory.com/26)  
[스프링 AOP CGLIB PROXY 방식 동작 이유](https://multifrontgarden.tistory.com/282)  
