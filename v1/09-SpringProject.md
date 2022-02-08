
>**WAS란**    

![WAS](https://t1.daumcdn.net/cfile/tistory/999FA3335B7E4A5924)  

**1)Web Server**    
-SW : 정적인 페이지(html, jpg 등)을 표현하기 위한 서버로 클라이언트의 요청에 따른 응답을 해주는 역할을 수행  

**2)Web Application Server (WAS)**   
-미들웨어 소프트웨어  
-웹브라우져의 요청(url)을 받아 웹 애플리케이션(CGI)를 호출하여 실행결과를 클라이언트에 보내주는 역할을 담당함  
-트랜잭션 처리, 쓰래드 관리, 동시성, 보안, DB(connection pool 등), 비용절감 등을 지원  
-CGI -> Servlet -> JSP, ASP, php 등으로 발전  
  
**3)WAS, WebServer**  
*종류  
 Was Server : tomcat, tMax jeus, BEA Web Logic, IBM Web Spere, JBOSS, Bluestone, Gemston, Inprise, Oracle, PowerTier, Apptivity, SilverStream  
 Web Server : IIS, apache, tMax WebtoB  

**4)WAS 기술 표준**    
J2EE : Java 기반의 분산객체 아키텍쳐, 쉽게 말해서 JAVA에 관련된 모듈의 표준규약  
(was => jsp/servlet, db => jdbc 등)  
WAS는 J2EE 아키텍쳐를 구현한 플랫폼 솔루션( = JSP/Servlet으로 구현되었다)  
WAS의 일반적인 기능, Web 환경을 위한 n-tier Architecture 플랫폼  

**5)다양한 WAS Framework**  
  
**EJB (Entertainment JAVA BEAN)**   
-분산환경을 지원하기 위한 J2EE기술  
-EJB 컨테이너의 다양한 기능지원 : 객체관리, 트랜잭션, 보안, 데이터베이스 등  
-다양한 기능으로 인한 퍼포먼스 저하  
-대형 기업 어플리케이션 시장, 금융권에서 사용됨  

**Spring framework**    
-로드 존슨이 만든기법   
-EJB서버와 같은 거창한 컨테이너가 필요없다  
-오픈소스 프레임웍이라서 사용이 무료  
-각종 기업용 어플리케이션 개발에 필요한 상당히 많은 라이브러리가 지원  
-스프링프레임웍은 모든 플랫폼에서 사용이 가능한 프레임웍이다  
-스프링은 웹분야 뿐만이 아니라 어플리케이션 모든 분야에 걸쳐 적용이 가능한 다양한 라이브러리를 가지고 있다.  


<br/>

>**Sping Cloud 적용한 마이크로 서비스 환경**  

![springcloud](https://t1.daumcdn.net/cfile/tistory/991F8C475C5C243320)

: Spring Cloud는 분산 시스템에서 공통적인 패턴(구성 관리, 서비스 검색, 지능형 라우팅, 마이크로 프록시 등 )을 모아 신속하게 구축할 수 있는 도구를 스프링 라이브러리 형태로 제공한다.  

따라서 개발자는 분산 시스템에서 필요한 부분들에 대한 부담을 덜고 충실하게 서비스의 기능을 구현하는 것에 충실할 수 있다. 또한 특정 벤더(AWS, Cloud Foundry 등)에 종속적이지 않기 때문에 다양한 분산 환경에서 잘 작동한다.   


```
Spring Config : Spring Boot Application은 application.properties 혹은 application.yml 파일에 환경설정을 저장하고 이 파일의 정보를 읽어 빌드하는데, 이 파일들은 해당 프로젝트와 함께 저장된다. 
Spring Cloud Config 서버를 두어 사용하면 모든 Spring Boot Application의 환경설정 파일을 한 곳에 저장시킬 수 있고 해당 서버에 접근하여 환경설정 정보를 가져오도록 할 수 있다.
이렇게 적용하면 모든 Application의 환경설정 정보를 한곳에서 관리가 가능하고 환경설정이 바뀌어도 Application 전체를 다시 빌드하지 않아도 된다. 
하지만 우리 팀은 현재 Spring Config를 적용하지 않았다. 개발 초기에 적용했을 당시 개발 중에 환경설정이 바뀌면 바뀐 내용을 Spring Cloud Config 서버에 반영해야 해서 번거롭고 확인도 다시 해당 서버에서 해야하는 점이 불편하였다.

RabbitMQ : AMQP (Advanced Message Queueing Protocol) 로 만들어져 있으며 Message Queue를 제공한다. 
이상적인 마이크로서비스 환경은 마이크로서비스 사이의 통신이 비동기적으로 이루어지는 것인데, RabbitMQ를 사용하면 마이크로 서비스들이 외부의 Queue를 통해 메세지를 주고받도록 함으로써 쉽게 이 부분을 구성할 수 있다. 

Eureka : 마이크로서비스들의 정보를 레지스트리에 등록할 수 있도록 하고 마이크로서비스의 동적인 탐색과 로드밸런싱을 제공한다.
```

<br/>  



>**References**  

[WAS](https://enderbridge.tistory.com/37)  
[Bean의 생명주기와 Bean Scope](https://maenco.tistory.com/entry/Spring-Container%EC%8A%A4%ED%94%84%EB%A7%81-%EC%BB%A8%ED%85%8C%EC%9D%B4%EB%84%88-Bean)  
[SpringCloud](https://lion-king.tistory.com/11)  

