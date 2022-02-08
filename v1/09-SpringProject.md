
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

**>Sping Cloud 적용한 마이크로 서비스 환경**  

![springcloud](https://t1.daumcdn.net/cfile/tistory/991F8C475C5C243320)



<br/>  



>**References**  

[WAS](https://enderbridge.tistory.com/37)  
[Bean의 생명주기와 Bean Scope](https://maenco.tistory.com/entry/Spring-Container%EC%8A%A4%ED%94%84%EB%A7%81-%EC%BB%A8%ED%85%8C%EC%9D%B4%EB%84%88-Bean)  
[SpringCloud](https://lion-king.tistory.com/11)  

