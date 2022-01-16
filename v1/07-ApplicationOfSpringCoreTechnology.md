
>**스프링 핵심 기술의 응용**  

![spring data access](https://mblogthumb-phinf.pstatic.net/MjAxODA3MjhfMTYw/MDAxNTMyNzcwNTM0NTY3.PO1f6Mwzv8YEQlTZ0_aQkrYVODbB7muc13Wh2hKzqgAg.fFi8YzLdiIrpMhf3nq2-CV-3S0GxwDEZM5TRRLiTf0Mg.PNG.good_ray/2_10_1___.png?type=w800)  

**-OXM이란**  
: XML과 자바 오브젝트를 매핑해서 상호 변환해주는 기술을 OXM(Object-XML Mapping) 이라고 합니다.  

**-ORM이란**  
: 객체와 관계형 데이터베이스의 데이터를 자동으로 매핑(연결)해주는 것을 ORM(Object-Relational Mapping) 이라고 합니다.   
-> **JPA(인터페이스)**, **Hibernate(jpa의 구현체)**, **Querydsl**(정적 타입을 이용해서 SQL과 같은 쿼리를 생성할 수 있도록 해 주는 비표준 오픈소스 프레임워크로 JPQL을 편하게 작성할 수 있게 만든 빌더 클래스 모음)  

**-MyBatis**  
: 개발자가 지정한 SQL, 저장프로시저 그리고 몇가지 고급 매핑을 지원하는 퍼시스턴스 프레임워크 Java Persistence Framework 중 하나 입니다. 
퍼시스턴스 프레임워크란 데이터의 저장, 조회, 수정, 삭제를 다루는 클래스 및 설정파일들의 집합을 말합니다.  

**-JDBC**  
: JDBC Connectivty 의 약자로 자바에서 데이터베이스 프로그래밍을 하기 위해 사용하는 API로써, JDBC드라이버를 연동하여 데이버베이스에 접근이 가능하다.  

**-JMS**  
: Java Message Service이다. [참고](https://unabated.tistory.com/entry/JNDI-JTA-JTS-JMS)  

<br/>

>**인터페이스 분리 원칙**  

: 인터페이스를 사용하는 클라이언트 기준으로 분리해야 되는 원칙.[예제참고](https://brownbears.tistory.com/580)  
Vehicle, Bicycle, Flight, Car method 예제 생각.    
Vehicle 인터페이스 하나로 구현한다면, drive, fly, ride를 전부 구현해야해서 원칙에 위반된다.  
AbstractBicycle, AbstractFlight, AbstractCar 역할별로 인터페이스를 만들어서 구분해서 구현 하는 방법이 있다.    

>**Multi Thread 환경에서 동시성 제어를 하는 방법**   

>**내장형 db h2 활용 방법**     


<br/>

>**References**  

[스프링이론기초 oxm, orm](https://m.blog.naver.com/good_ray/221328422224)  
[JNDI, JTA, JTS, JMS](https://unabated.tistory.com/entry/JNDI-JTA-JTS-JMS)  
[JPA와 MYBATIS 차이](https://dreaming-soohyun.tistory.com/entry/JPA%EC%99%80-MyBatis%EC%9D%98-%EC%B0%A8%EC%9D%B4-ORM%EA%B3%BC-SQL-Mapper)  
[자바 퍼시스턴스 프레임워크란](https://jiwontip.tistory.com/57)  
[Spring Data Jpa, Jpa, Hibernate 차이](https://suhwan.dev/2019/02/24/jpa-vs-hibernate-vs-spring-data-jpa/)  
[우아한형제들의 QueryDsl 사용법](https://velog.io/@youngerjesus/%EC%9A%B0%EC%95%84%ED%95%9C-%ED%98%95%EC%A0%9C%EB%93%A4%EC%9D%98-Querydsl-%ED%99%9C%EC%9A%A9%EB%B2%95)  
[PostConstruct(DI까지 완료 후 초기화)와 빈 생명주기 메서드와 실행순서](https://madplay.github.io/post/spring-bean-lifecycle-methods)  
[인터페이스 분리 원칙 - ISP](https://brownbears.tistory.com/580)   
[ConcurrentHashMap이란](https://devlog-wjdrbs96.tistory.com/269)  
[Multi Thread 환경에서 동시성 제어하는 방법](https://deveric.tistory.com/104)  
