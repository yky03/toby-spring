
# 01. 오브젝트와 의존 관계

> **스프링이란**   
: 스프링 프레임워크(영어: Spring Framework)는 자바 플랫폼을 위한 오픈 소스 애플리케이션 **경량화 프레임워크**

<img src="https://spring.io/images/spring-logo-9146a4d3298760c2e7e49595184e1975.svg" width="400" height="100"/>

- 스프링의 특징 7가지  

1) **경량 컨테이너로**서 자바 객체를 직접 관리한다. 각각의 객체 생성, 소멸과 같은 라이프 사이클을 관리하며 스프링으로부터 필요한 객체를 얻어올 수 있다.
2) 스프링은 **Plain Old Java Object 방식의 프레임워크**이다. 일반적인 J2EE 프레임워크에 비해 구현을 위해 **특정한 인터페이스를 구현하거나 상속을 받을 필요가 없어** 기존에 존재하는 라이브러리 등을 지원하기에 용이하고 객체가 가볍다.
3) 스프링은 제어 반전(**IoC** : Inversion of Control)을 지원한다. 컨트롤의 제어권이 사용자가 아니라 프레임워크에 있어서 필요에 따라 스프링에서 사용자의 코드를 호출한다.
4) 스프링은 의존성 주입(**DI** : Dependency Injection)을 지원한다. 각각의 계층이나 서비스들 간에 의존성이 존재할 경우 프레임워크가 서로 연결시켜준다.
5) 스프링은 관점 지향 프로그래밍(**AOP** : Aspect-Oriented Programming)을 지원한다. 따라서 트랜잭션이나 로깅, 보안과 같이 여러 모듈에서 공통적으로 사용하는 기능의 경우 해당 기능을 분리하여 관리할 수 있다.
6) 스프링은 **영속성**과 관련된 다양한 서비스를 지원한다. iBATIS나 하이버네이트 등 이미 완성도가 높은 데이터베이스 처리 라이브러리와 연결할 수 있는 인터페이스를 제공한다.
7) 스프링은 **확장성**이 높다. 스프링 프레임워크에 통합하기 위해 간단하게 기존 라이브러리를 감싸는 정도로 스프링에서 사용이 가능하기 때문에 수많은 라이브러리가 이미 스프링에서 지원되고 있고 스프링에서 사용되는 라이브러리를 별도로 분리하기도 용이하다.

> 빈과 빈 팩토리 ( 애플리케이션 컨텍스트 )  
: 스프링의 핵심을 담당하는 건 빈 팩토리 또는 애플리케이션 컨텍스트라고 불리는 것이다.

**빈(bean)이란 스프링이 제어권을 가지고 직접 만들고 관계를 부여하는 오브젝트**를 말하며, 오브젝트 단위의 애플리케이션 **컴포넌트**라고도 한다. 

동시에 **스프링 빈은 스프링 컨테이너가 생성과 관계설정, 사용 등을 제어해주는 제어의 역전이 적용된 오브젝트**를 가리키는 말이다.

스프링에서는 빈의 생성과 관계설정 같은 제어를 담당하는 IoC 오브젝트를 빈 팩토리(bean factory)라고 부른다.

보통 **빈 팩토리보다는 이를 좀 더 확장한 애플리케이션 컨텍스트(application context)를 주로 사용**한다.

애플리케이션 컨텍스트는 별도의 정보를 참고해서 빈(오브젝트)의 생성, 관계설정 등의 제어 작업을 총괄한다.


> 개방 폐쇄 원칙을 따르자 ( SOLID 객체지향 설계 원칙도 항상 기억 )  
: 클래스나 모듈은 확장에는 열려 있어야 하고 변경에는 닫혀 있어야 한다는 원칙이다.
소프트웨어 공학적으로 스프링 프레임워크를 활용하여 좀 더 쉽게 응집도는 높고 결합력은 약하게 만들 수 있다.

> 스프링에서 주입은 생성자주입 방식으로 사용하자  
final사용가능, 순환참조방지(서버실행시 바로 에러 파악 가능), 테스트코드 작성 용이.
-> **필드주입과 수정자 주입**은 먼저 **빈을 생성한 후, 주입하려는 빈을 찾아 주입**한다.
-> **But 생성자 주입**은 먼저 생성자의 인자에 사용되는 빈을 찾거나 빈 팩토리에서 만든다. 그 후 찾은 인자 빈으로 주입하려는 빈의 생성자를 호출한다.
즉, **먼저 빈을 생성하지 않고 주입하려는 빈을 먼저 찾는다.**


> 디자인패턴 3가지(1장에서는 Dao 마다  DBConnection 객체를 생성하는 부분을 관심사의 분리와 빈 주입을 통해 리팩토링해 나가는 예제 )  
**1) 템플릿 메소드 패턴**  
: 슈퍼 클래스에 기본적인 로직의 흐름을 만들고, 그 기능의 일부를 추상 메소드나 오버라이딩이 가능한 protect 메소드 등으로 만든 뒤 서브 클래스에서 이런 메소드를 필요에 맞게 구현해 사용하도록 하는 방법. -> 즉 **슈퍼 클래스에 기본적인 메소드가 정의되어 있고 세부적인 메소드는 추상 메소드로 두어 서브 클래스에서 구현해 사용하는 방법**  
```java
package com.kjh.hojak.controller;

public class Main {
	public static void main(String[] args) {
		Chrome chrome = new Chrome();
		chrome.working();
		
		System.out.println();
		
		Ie ie = new Ie();
		ie.working();
	}

}

abstract class Work {
	public void working(){
		System.out.println("컴퓨터 킨다.");
		browser();
		System.out.println("검색을 한다.");
		System.out.println("컴퓨터를 끈다.");
	}
	
	public abstract void browser();
}

class Chrome extends Work {

	public void browser(){
		System.out.println("크롬을 킨다.");
	}
	
}

class Ie extends Work {
	
	public void browser(){
		System.out.println("인터넷 익스플로어를 킨다.");
	}
	
}
```

**2) 팩토리 메소드 패턴**  
:  서브클래스에서 구체적인 오브젝트 생성 방법을 결정하게 하는 것.( 객체 생성 등 예제 참고 )
```java
abstract class UserDao {
	public void add(User user) {
		Connection c = getConnection();
	}
	
	public User get(String id) {
		Connection c = getConnection();
	}
	
	public abstract Connection getConnection();
}


class OracleUserDao extends UserDao{
	public Connection getConnection(){
		// 오라클 connection 생성코드
	}
}

class MySqlUserDao extends UserDao{
	public Connection getConnection(){
		// MySql connection 생성코드
	}
}

```

**3) 전략패턴**  
: 자신의 기능 맥락(context)에서, 필요에 따라 변경이 필요한 알고리즘을 인터페이스를 통해 통째로 외부로 분리시키고, 이를 구현한 구체적인 알고리즘 클래스를 필요에 따라 바꿔서 사용할 수 있게 하는 디자인 패턴이다.  
ex) UserDao는 전략 패턴의 컨텍스트에 해당한다. UserDaoTest 클라이언트는 컨텍스트가 사용할 전략을 바꿔가면서 사용할 수 있다. (예제 참고)  


** *Tip **  
**퍼시스턴스 프레임워크**는 데이터의 저장, 조회, 변경, 삭제를 다루는 클래스 및 설정 파일들의 집합이다.    
Spring IoC 컨테이너가 관리하는 자바 객체를 **빈(Bean)**이라 한다.  
**싱글톤 레지스트리**란 싱글톤 패턴을 사용함으로써 발생되는 문제점들을 보완하기 위해 스프링 프레임워크에서 직접 싱글톤을 생성, 관리 해주는 기능이다.(싱글톤 오브젝트는 멀티스레드 환경에서 동시에 접근이 가능하므로 상태정보를 내부에 갖고 있지 않은 무상태 방식으로 만들어져야함)


> 1장 오브젝트와 의존 관계 정리   
- 먼저 책임이 다른 코드를 분리해서 두 개의 클래스로 만들었다.(관심사의분리-SoC, 리팩토링)  
 
- 그중에서 바뀔 수 있는 쪽의 클래스는 인터페이스를 구현하도록 하고, 다른 클래스에서 인터페이스를 통해서만 접근하도록 만들었다. 이렇게 해서 인터페이스를 정의한 쪽의 구현 방법이 달라져 클래스가 바뀌더라도, 그 기능을 사용하는 클래스의 코드는 같이 수정할 필요가 없도록 만들었다.(전략 패턴)  

- 이를 통해 자신의 책임 자체가 변경되는 경우 외에는 불필요한 변화가 발생하지 않도록 막아주고, 자신이 사용하는 외부 오브젝트 기능은 자유롭게 확장하거나 변경할 수 있게 만들었다.(개방 폐쇄 원칙)  

- 결국 한쪽의 기능변화가 다른 쪽의 변경을 요구하지 않아도 되게 했고(낮은결합도), 자신의 책임과 관심사에만 순수하게 집중하는(높은 응집도) 깔끔한 코드를 만들 수 있었다.  

- 오브젝트가 생성되고 여타 오브젝트와 관계를 맺는 작업의 제어권을 별도의 오브젝트 팩토리를 만들어 넘겼다. 또는 오브젝트 팩토리의 기능을 일반화한 IoC 컨테이너로 넘겨서 오브젝트가 자신이 사용할 대상의 생성이나 선택에 관한 책임으로부터 자유롭게 마들어줬다(제어의역전-IoC)  

- 전통적인 싱글톤 패턴 구현 방식의 단점을 살펴보고, 서버에서 사용되는 서비스 오브젝트로서의 장점을 살릴 수 있는 싱글톤을 사용하면서도 싱글톤 패턴의 단점을 극복할 수 있도록 설계된 컨테이너를 활용하는 방법에 대해 알아봤다(싱글톤 레지스트리)  

- 설계 시점과 코드에는 클래스와 인터페이스 사이의 느슨한 의존관계만 만들어놓고, 런타임 시에 실제 사용할구체적인 의존 오브젝트를 제3자(DI컨테이너)의 도움으로 주입받아서 다이내믹한 의존관계를 갖게 해주는  IoC의 특별한 케이스를 알아봤다.(의존관계 주입-DI)  

- 의존오브젝트를 주입할 때 생성자를 이용하는 방법과 수정자 메소드를 이용하는 방법을 알아봤다(생성자 주입과 수정자 주입)  

- 마지막으로, XML을 이용해 DI 설정정보를 만드는 방법과 의존 오브젝트가 아닌 일반 값을 외부에서 설정하게 런타임 시에 주입하는 방법을 알아봤다.(XML 설정)  


** 추가적으로.. Reference 참고 **
Spring Boot로 전환해야 하는 이유에 대해서..  
Maven과 Gradle 에 대해서..  


# Reference
[wiki-스프링이란](https://ko.wikipedia.org/wiki/%EC%8A%A4%ED%94%84%EB%A7%81_%ED%94%84%EB%A0%88%EC%9E%84%EC%9B%8C%ED%81%AC) 
[빈과 빈팩토리 - 애플리케이션 컨텍스트](https://hardlearner.tistory.com/342)  
[필드주입 대신 생성자주입 방식을 사용하자](https://jackjeong.tistory.com/41)  
[Autowired, Resource, Inject 차이](https://devmg.tistory.com/143)  
[디자인패턴-팩토리메소드패턴 템플릿메소드패턴 차이](https://hojak99.tistory.com/347)  
[디자인패턴-전략패턴&디비커넥션예제](https://withseungryu.tistory.com/68)  
[SOLID 객체지향 설계 원칙](https://velog.io/@zayson/Spring-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8-3-SOLID-%EC%9B%90%EC%B9%99)  
[토비스프링 1장 정리](https://kurts.tistory.com/11)  
[Spring Boot를 사용해야 하는 이유](https://yjh5369.tistory.com/entry/spring-boot%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%B4%EC%95%BC-%ED%95%98%EB%8A%94-%EC%9D%B4%EC%9C%A0)   
[maven vs gradle](https://jisooo.tistory.com/entry/Spring-%EB%B9%8C%EB%93%9C-%EA%B4%80%EB%A6%AC-%EB%8F%84%EA%B5%AC-Maven%EA%B3%BC-Gradle-%EB%B9%84%EA%B5%90%ED%95%98%EA%B8%B0)  
[Maven With Nexus](https://dev-youngjun.tistory.com/105)  
