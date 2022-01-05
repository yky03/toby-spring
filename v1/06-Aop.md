
>**AOP**  

: AOP는 Aspect Oriented Programming의 약자로 관점 지향 프로그래밍이라고 불린다.  
**관점 지향은 쉽게 말해 어떤 로직을 기준으로 핵심적인 관점, 부가적인 관점으로 나누어서 보고 그 관점을 기준으로 각각 모듈화**하겠다는 것이다.  
여기서 모듈화란 어떤 공통된 로직이나 기능을 하나의 단위로 묶는 것을 말한다.  


<br/>

>**AOP의 주요 개념**  

**Aspect** : **여러 객체에 공통적으로 적용되는 관심사**항으로써 위에서 설명한 흩어진 관심사를 모듈화 한 것. 주로 부가기능을 모듈화함. 
(Advice + PointCut = Aspect)  
**Target** : Aspect를 적용하는 곳 (클래스, 메서드 .. )  
**Advice** : 실질적으로 어떤 일을 해야할 지에 대한 것, 실질적인 부가기능을 담은 구현체  
**JointPoint** : 프로그램이 실행 중일 때 발생하는 메서드 실행, 생성자 호출, 필드 값 변경과 같은 특수한 지점을 의미함.  
(Advice가 적용될 위치, 끼어들 수 있는 지점. 메서드 진입 지점, 생성자 호출 시점, 필드에서 값을 꺼내올 때 등 다양한 시점에 적용가능)   
**PointCut** : JointPoint의 상세한 스펙을 정의한 것(정규표현식). 'A란 메서드의 진입 시점에 호출할 것'과 같이 더욱 구체적으로 Advice가 실행될 지점을 정할 수 있음  
(JoinPoint가 Pointcut에 일치할 때마다 해당 Pointcut에 관련된 Advice가 실행됨.)  
**Weaving** : Aspect를 대상 객체에 연결시켜 관점지향 객체로 만드는 과정을 의미함.  
(Advice를 비즈니스 로직 코드에 삽입하는것을 의미함)  
**Advisor** : SpringAOP에서만 사용되는 특별한 용어로 Advice + PointCut  


<br/>


>**Spring AOP와 AspectJ 비교하기**  

Spring Aop와 AspectJ는 목적 자체가 다름.    

**Spring Aop**는 개발자가 마주한 공통적인 문제를 해결하고자 Spring IOC를 통한 **간단한 AOP 구현**이 그 목적.    
완전한 AOP를 의도한 것이 아니며, Spring Container에 의해 관리되는 **Beans에만 적용**이 가능함.  
Proxy 기반 AOP 프레임 워크이며 메서드 조인 포인트만 지원함.(JoinPoint가 메서드 호출 시점도 안되고 메서드 실행시에만 지원됨.)  
AspectJ에 비해 훨씬 느리지만 배우고 적용하기 쉽다.    

**반면 AspectJ** 는 완전한 AOP를 제공하는 것이 목적인 근원적인 AOP기술.  
Spring AOP보다 복잡하긴 하지만 모든 객체에 적용이 가능하고 성능이 더 좋다.     

AspectJ는 세가지 방식의 Weaving을 사용할 수 있습니다.  
-컴파일 시점 Weaving  
-컴파일 후 Weaving  
-로드시점 Weaving  

Spring Aop는 런타임 Weaving을 사용합니다.  
-런타임 시점 Weaving  


<br/>


>**프록시패턴**  

: Proxy는 우리말로 대리자, 대변인 이라는 뜻으로 프록시에게 어떤 일을 대신 시키는 것입니다.  
객체에 직접 접근하지 않고 앞단에 프록시를 하나 둠으로써 시큐리티 로직, 로깅 등의 로직을 공통으로 넣기가 편합니다.  

-proxy example  
```java
//IService.java
public interface IService {
	String runSomething();
}
```
```java
//Service.java
public class Service implements IService {
	@Override
	public String runSomething() {
		return "서비스 짱!!!";
	}
}
```
```java
//Proxy.java
public class Proxy implements IService {
	IService service1;

	@Override
	public String runSomething() {
		System.out.println("호출에 대한 흐름 제어가 주목적, 반환 결과를 그대로 전달");
		service1 = new Service();
		return service1.runSomething();
	}
}
```
```java
//Main.java
public class Main {  	
public static void main(String[] args) { 		
	//직접 호출하지 않고 프록시를 호출한다. 		
	IService proxy = new Proxy(); 		
	System.out.println(proxy.runSomething()); 	
	}
}
}
```

인터페이스를 중간에 두어 구체 클래스들에게 영향을 받지 않도록 설계하였고,  
직접 접근하지 않고 Proxy를 통해서 한번 더 우회해서 접근하도록 객체 지향적으로 설계.    


<br/>



>**데코레이터패턴**  

: 데코레이터 패턴(Decorator pattern)이란 주어진 상황 및 용도에 따라 어떤 객체에 책임을 **덧붙이는 패턴**으로,  
기능 확장이 필요할 때 서브클래싱 대신 쓸 수 있는 유연한 대안이 될 수 있다.  

-decorator example    
```java
// the Window interface
interface Window {
    public void draw(); // draws the Window
    public String getDescription(); // returns a description of the Window
}

// implementation of a simple Window without any scrollbars
class SimpleWindow implements Window {
    public void draw() {
        // draw window
    }

    public String getDescription() {
        return "simple window";
    }
}
```
```java
// abstract decorator class - note that it implements Window
abstract class WindowDecorator implements Window {
    protected Window decoratedWindow; // the Window being decorated

    public WindowDecorator (Window decoratedWindow) {
        this.decoratedWindow = decoratedWindow;
    }
}

// the first concrete decorator which adds vertical scrollbar functionality
class VerticalScrollBarDecorator extends WindowDecorator {
    public VerticalScrollBarDecorator (Window decoratedWindow) {
        super(decoratedWindow);
    }

    public void draw() {
        drawVerticalScrollBar();
        decoratedWindow.draw();
    }

    private void drawVerticalScrollBar() {
        // draw the vertical scrollbar
    }

    public String getDescription() {
        return decoratedWindow.getDescription() + ", including vertical scrollbars";
    }
}

// the second concrete decorator which adds horizontal scrollbar functionality
class HorizontalScrollBarDecorator extends WindowDecorator {
    public HorizontalScrollBarDecorator (Window decoratedWindow) {
        super(decoratedWindow);
    }

    public void draw() {
        drawHorizontalScrollBar();
        decoratedWindow.draw();
    }

    private void drawHorizontalScrollBar() {
        // draw the horizontal scrollbar
    }

    public String getDescription() {
        return decoratedWindow.getDescription() + ", including horizontal scrollbars";
    }
}
```
```java
public class DecoratedWindowTest {
    public static void main(String[] args) {
        // create a decorated Window with horizontal and vertical scrollbars
        Window decoratedWindow = new HorizontalScrollBarDecorator (
                new VerticalScrollBarDecorator(new SimpleWindow())); // 데코 형태

        // print the Window's description
        System.out.println(decoratedWindow.getDescription());
    }
}
```


<br/>


>**Q&A**  

[Filter, Interceptor, Aop 에 대한 이해](https://goddaehee.tistory.com/154)  
[다이내믹 프로시 패턴과 리플렉션에 대한 이해](https://codechacha.com/ko/reflection/)  
[위빙에 대한 정확한 이해](https://jaehun2841.github.io/2018/07/22/2018-07-22-spring-aop4/)  
: 위빙(Weaving) 이란? Aspect 클래스에 정의 한 Advice 로직을 타깃(Target)에 적용하는 것을 의미한다. 위빙 방법으로는 RTW, CTW, LTW 3가지가 있다.  
[@Around Example](https://howtodoinjava.com/spring-aop/aspectj-around-annotation-example/)  
: Spring Aop 는 @Transactional, @Cacheable 을 실무에서 많이 쓰며, AspectJ는 @Aspect @Around Custom Annotation 으로 로깅 어노테이션을 구현하여 활용이 가능하다.  


![AOP흐름](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile22.uf.tistory.com%2Fimage%2F9983FB455BB4E5D30C7E10)  



>**References**  

[토비스프링 AOP 정리](https://haviyj.tistory.com/26?category=695904)  
[우아한형제들 AOP](https://www.youtube.com/results?search_query=%EC%9A%B0%EC%95%84%ED%95%9C%ED%98%95%EC%A0%9C%EB%93%A4+aop)  
[스프링 AOP 총정리](https://engkimbs.tistory.com/746)  
[프록시패턴 정의](https://limkydev.tistory.com/79)  
[백기선님강의-프록시패턴](https://www.youtube.com/watch?v=tes_ekyB6U8)  
[Enable Aspectj document](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#aop-aspectj-support)  
[Spring AOP 와 AspectJ 비교](https://logical-code.tistory.com/118)  
[데코레이터 패턴](https://ko.wikipedia.org/wiki/%EB%8D%B0%EC%BD%94%EB%A0%88%EC%9D%B4%ED%84%B0_%ED%8C%A8%ED%84%B4)  

