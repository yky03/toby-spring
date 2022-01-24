
>**스프링이란 무엇인가**  

: 자바 엔터프라이즈 개발을 편하게 해주는 오픈소스 경량급 애플리케이션 프레임워크.  

<br/>

>**경량화**  

-스프링 자체가 가볍거나 작은 규모의 코드로 이루어진 것은 아니다.  

-오히려 스프링은 20여개의 모듈로 세분화되고 복잡하고 방대한 코드를 가진 프레임워크이다.  

-경량화가 특징인 이유는 기존 자바 엔터프라이즈 기술의 불필요한 복잡함에 반대되는 개념에서 시작되었다.  

-주류 기술이었던 EJB는 고가의 무거운 자바 서버(WAS)가 필요했고, 다루기 힘든 설정파일 구조, 패키징, 불편한 배포 등이 단점이었다.  

-반면, 스프링은 톰캣과 같은 단순한 서버환경에서도 동작하며, 단순한 개발환경으로도 엔터프라이즈 애플리케이션 개발하는데 충분하다.  

-또 EJB 등의 기존 프레임워크에서 만들어진 코드에 비해 코드량이 적고 단순하기도 하다.  

-즉, 기존에 비해 빠르고 간편하게 애플리케이션을 개발할 수 있어 생산성이 뛰어난 프레임워크다.  

<br/>

>**스프링 기능**  

-경량 컨테이너로서 자바 객체를 직접 관리  
-> 각각의 객체 생성, 소멸과 같은 라이프 사이클을 관리하며 스프링으로부터 필요한 객체를 얻어올 수 있다.  

-POJO(Plain Old Java Object) 방식의 프레임워크
->일반적인 J2EE 프레임워크에 비해 구현을 위해 특정한 인터페이스를 구현하거나 상속을 받을 필요가 없어 기존에 존재하는 라이브러리 등을 지원하기에 용이하고 객체가 가볍다.  

-제어 반전(또는 제어의 역전)(IoC: Inversion of Control)을 지원  
->컨트롤의 제어권이 사용자가 아니라 프레임워크에 있어서 필요에 따라 스프링에서 사용자의 코드를 호출한다.  

-의존성 주입(DI: Dependency Injection)을 지원  
->각각의 계층이나 서비스들 간에 의존성이 존재할 경우 프레임워크가 서로 연결시켜준다.  

-관점 지향 프로그래밍(AOP: Aspect-Oriented Programming)을 지원  
->트랙잭션이나 로깅, 보안과 같이 여러 모듈에서 공통적으로 사용하는 기능의 경우 해당 기능을 분리하여 관리할 수 있다.  

-확장성이 높음  
->스프링 프레임워크에 통합하기 위해 간단하게 기존 라이브러리를 감싸는 정도로 스프링에서 사용이 가능하다.  
->수많은 라이브러리가 이미 스프링에서 지원되고 있고 스프링에서 사용되는 라이브러리를 별도로 분리하기도 용이하다.  

-다른 프레임워크들의 통합  

-오픈소스  
-> 스프링은 아파치 라이선스 2.0을 따르므로 무료이다.  

<br/>

>**SpringBootApplication Annotation 분석**  

-SpringdatademoApplication.java  
```java
@SpringBootApplication
public class SpringdatademoApplication {
    public static void main(String[] args) {
        SpringApplication.run(SpringdatademoApplication.class, args);
    }
}
```

-SpringBootApplication.class  
```java
//
// Source code recreated from a .class file by IntelliJ IDEA
// (powered by FernFlower decompiler)
//
package org.springframework.boot.autoconfigure;

import java.lang.annotation.Documented;
import java.lang.annotation.ElementType;
import java.lang.annotation.Inherited;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;
import org.springframework.beans.factory.support.BeanNameGenerator;
import org.springframework.boot.SpringBootConfiguration;
import org.springframework.boot.context.TypeExcludeFilter;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.FilterType;
import org.springframework.context.annotation.ComponentScan.Filter;
import org.springframework.core.annotation.AliasFor;

@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(
    excludeFilters = {@Filter(
    type = FilterType.CUSTOM,
    classes = {TypeExcludeFilter.class}
), @Filter(
    type = FilterType.CUSTOM,
    classes = {AutoConfigurationExcludeFilter.class}
)}
)
public @interface SpringBootApplication {
    @AliasFor(
        annotation = EnableAutoConfiguration.class
    )
    Class<?>[] exclude() default {};

    @AliasFor(
        annotation = EnableAutoConfiguration.class
    )
    String[] excludeName() default {};

    @AliasFor(
        annotation = ComponentScan.class,
        attribute = "basePackages"
    )
    String[] scanBasePackages() default {};

    @AliasFor(
        annotation = ComponentScan.class,
        attribute = "basePackageClasses"
    )
    Class<?>[] scanBasePackageClasses() default {};

    @AliasFor(
        annotation = ComponentScan.class,
        attribute = "nameGenerator"
    )
    Class<? extends BeanNameGenerator> nameGenerator() default BeanNameGenerator.class;

    @AliasFor(
        annotation = Configuration.class
    )
    boolean proxyBeanMethods() default true;
}

```

<br/>

*중요 어노테이션 -> @ComponentScan, @EnableAutoConfiguration  
( 왜냐하면 Bean이 @ComponentScan과 @EnableAutoConfiguration에 의해 두 단계로 나눠서 읽히기 떄문 )  


@ComponentScan
```
@Configuration, @Repository, @Controller, @Service, @RestController
위 어노테이션을 포함하는 모든 클래스를 찾아서 Bean으로 등록해준다.
```

@EnableAutoConfiguration
```
Spring의 META-INF 파일을 읽어들인다.
META-INF 아래 spring.factories 파일이 존재하는데 org.springframework.boot.autoconfigure.EnableAutoConfiguration 를 키값으로 하는 모든 설정을 가져온다.
실제로 전부 다 가져오는게 아니고 @ConditionalOn~~ 조건에 따라 다르다.
```

<br/>

즉, ComponentScan은 안에 해당되는 어노테이션에 대한 설정을 Bean으로 등록하여 사용 가능하게 해주고 EnableAutoConfiguration은 환경 설정에 필요한 자동 설정을 Bean으로 등록해준다.  

따라서 EnableAutoConfiguration에 있는 설정을 사용하지 않을 것이라면 사용하지 않고 @Configuration, @ComponentScan 만으로도 구동이 가능하다.  


<br/>

>**References**  

[스프링이란](https://hoonmaro.tistory.com/32)  
[@SpringBootApplication](https://n1tjrgns.tistory.com/231)  
[스프링 어노테이션 총정리](https://velog.io/@gillog/Spring-Annotation-%EC%A0%95%EB%A6%AC)  
[롬복이란](https://bcp0109.tistory.com/224)  
