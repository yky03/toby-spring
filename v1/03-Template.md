
>**템플릿**    

: **스프링에 적용된 템플릿 기법을 살펴보고, 이를 적용해 완성도 있는 DAO 코드를 만드는 방법을 알아보자.**  

2장에서는 DBConnection 에 대한 예제로 1장에서 다뤘던 아래의 디자인 패턴들로 try-catch-finally 패턴에서 close   
즉, 예외발생시 리소스를 제대로 반납하지 않아 커넥션풀 문제로 서버가 다운될 수 있는 상황에 대해 설명하고 있다.   

<br/>

>**디자인패턴 & 리팩토링**  

**-개방 폐쇄 원칙**   
: 확장에는 자유롭게 열려 있고 변경에는 굳게 닫혀 있다는 객체지향 설계의 핵심 원칙이다.  
<br/>
**-메소드 추출**  
: 변화되거나 공통되는 부분을 메소드로 빼서 가독성 및 재사용이 가능하도록 한다.  

**-템플릿 메소드 패턴 & 전략 패턴 example**
: https://github.com/yky03/toby-spring/blob/main/v1/01-ObjectAndDependencyRelationship.md
<br/>

**-DI 적용을 위해 클라이언트/컨텍스트를 분리하고 전략클래스, 로컬클래스, 익명 내부 클래스(람다로 사용 가능)를 활용하자.**   

<br/>


>**Tip**  

: try catch 보다는 try with resource를 사용하라! -> [이펙티브 자바](https://sabarada.tistory.com/78)  

**[AS-IS]**   
-> [Bubble Style](https://soft.plusblog.co.kr/164)로 가독성 떨어짐.  
```java
finally {
    if(bufferedInputStream != null) {
        try {
            bufferedInputStream.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    try {
        if(fileInputStream == null) {
            fileInputStream.close();
        }
    } catch (IOException e) {
        e.printStackTrace();
    }
}
```
**[TO-BE]**  
-> 복잡한 구문 없이 자원 반납 가능.   
```java
try (BufferedInputStream bufferedInputStream = new BufferedInputStream(new FileInputStream(file))){
    ...
} catch ( ) {
    ...
}
```


--------------------------- p.231 ---------------------------
