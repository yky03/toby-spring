
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
: Java Database Connectivty 의 약자로 자바에서 데이터베이스 프로그래밍을 하기 위해 사용하는 API로써, JDBC드라이버를 연동하여 데이버베이스에 접근이 가능하다.  

**-JMS**  
: Java Message Service로써 비동기식 메시징을 위한 표준 API이다 목적지로는 큐와 토픽 모델이 있다.  

<br/>

>**인터페이스 분리 원칙**  

: 인터페이스를 사용하는 클라이언트 기준으로 분리해야 되는 원칙.[[예제참고]](https://brownbears.tistory.com/580)  
Vehicle, Bicycle, Flight, Car method 예제 생각.    
Vehicle 인터페이스 하나로 구현한다면, drive, fly, ride를 전부 구현해야해서 원칙에 위반된다.  
AbstractBicycle, AbstractFlight, AbstractCar 역할별로 인터페이스를 만들어서 구분해서 구현 하는 방법이 있다.    


>**ConcurrentHashMap 사용하기**  
일반적인 HashMap은 멀티쓰레드 환경에서 의도치 않은 결과가 발생할 수 있다. Collections.synchronizedMap()과 같이 외부에서 동기화해주는 메서드가 개발되어 있지만 모든 작업을 동기화하게 되면 많은 요청이 몰릴 때 성능 하락은 피할 수 없다. 대신 동기화된 해시데이터 조작에 최적화된 ConcurrentHashMap이 대안이 될 수 있다.  

전체 데이터에 락을 걸지 않는다.  
읽기 작업엔 락을 사용하지 않는다.  
동시성에 대한 테스트는 작성하기가 매우 어렵다. 대신 ConcurrentHashMap을 이용한 SqlRegistry 구현을 우선 만들고 테스트할 수 있다.  

내장형 데이터베이스
그러나 ConcurrentHashMap은 변경이 자주 일어나는 환경에선 동기화의 성능하락에서 자유로울 수 없다. 그래서 SQL을 담는 DB같은 것을 설계해볼 순 있지만, 관계형 데이터베이스 스키마를 구현하는 것은 배보다 배꼽이 더 커질 수가 있다. 그래서 [**내장형 DB(H2등)**](https://datamoney.tistory.com/214)를 고려해볼 수 있다.  

```java
@Sql("/generate_schema.sql")
public class MyTest {
}
```

내장형 DB(Embedded DB)는 인메모리 DB라고 생각하면 좋다. Persistence는 보장되지 않지만 메모리에 저장되어 빠른 IO가 가능하다. 또한 등록, 수정, 검색, 격리수준, 트랜잭션, 최적화된 락킹 등 DB가 제공할 수 있는 것들은 모두 제공할 수 있다. SQL문으로 질의가 가능한 것은 덤이다.  

**-추가학습**    
concurrent패키지에 존재하는 컬랙션들은 락을 사용할 때 발생하는 성능 저하를 최소한으로 만들어두었습니다. 락을 여러 개로 분할하여 사용하는 Lock Striping 기법을 사용하여 동시에 여러 스레드가 하나의 자원에 접근하더라도 동시성 이슈가 발생하지 않도록 도와줍니다. 

```java   
 int binCount = 0;
        for (Node<K,V>[] tab = table;;) {
            Node<K,V> f; int n, i, fh;
            if (tab == null || (n = tab.length) == 0)
                tab = initTable();
            else if ((f = tabAt(tab, i = (n - 1) & hash)) == null) {
                if (casTabAt(tab, i, null,
                             new Node<K,V>(hash, key, value, null)))
                    break;                   // no lock when adding to empty bin
            }
            else if ((fh = f.hash) == MOVED)
                tab = helpTransfer(tab, f);
            else {
                V oldVal = null;
                synchronized (f) {
                    if (tabAt(tab, i) == f) {
```

ConcurrentHashMap은 내부적으로 여러개의 락을 가지고 해시값을 이용해 이러한 락을 분할하여 사용합니다. 분할 락을 사용하여 병렬성과 성능이라는 두 마리의 토끼를 모두 잡은 컬랙션이라고 볼 수 있겠네요. 내부적으로 여러 락을 사용하지만 실제 구현이 어떻게 되어 있는지 자세히 알 필요는 없습니다. 일반적인 map을 사용할 때처럼 구현하면 내부적으로 알아서 락을 자동으로 사용해 줄 테니 편리하게 사용할 수 있습니다.  

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
[토비스프링정리](https://daebalpri.me/entry/%ED%86%A0%EB%B9%84%EC%9D%98-%EC%8A%A4%ED%94%84%EB%A7%81-31-7%EC%9E%A5-%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EA%B8%B0%EC%88%A0%EC%9D%98-%EC%9D%91%EC%9A%A9)  
