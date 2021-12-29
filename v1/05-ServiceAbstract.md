
>**서비스 추상화**  

: **추상화**란 하위 **시스템의 공통점을 뽑아내서 분리**시키는 것을 말한다. 그렇게 하면 하위 시스템이 어떤 것인지  
알지 못해도, 또는 하위 시스템이 바뀌더라도 **일관된 방법으로 접근**할 수가 있다.

[-서비스 추상화 챕터 정리](https://spring-com.tistory.com/11)  


<br/>

>**심화학습**  

[-enum](https://github.com/yky03/java-study-issues/blob/main/week11-enum.md)    
: 이늄이란 Eumeration 으로 열거형이라고 불리며, 서로 연관된 상수들의 집합을 의미한다.  

```
values() 메소드는 enum안에 존재하는 모든 값들을 반환한다.

enum Color {
  RED, GREEN, BLUE
}

public class Test{
    public static void main(String[] args) {

        Color arr[] = Color.values();

        for (Color color : arr) {
            System.out.println(color + " at index " + color.ordinal());
        }

        System.out.println(Color.valueOf("RED"));
    }
}

/*** 출력 
RED at index 0
GREEN at index 1
BLUE at index 2
RED
**/

-enum Method 종류
toString
name (toString + final 의 역할)
ordinal - 인덱스를 나타내며 사용하지 말것
valueOf
equals
hashCode
clone
compareTo
getDeclaringClass
finalize
```

[-transaction](http://jmlim.github.io/spring/2018/12/07/spring-transaction/)    
: 트랜잭션이란 데이터베이스 연산들의 논리적 단위이며 트랜잭션 내 모든 연산들이 정상적으로 완료되지 않으면 아무 것도 수행되지 않은 원래 상태로 복원되어야 한다.  

**- 트랜잭션 격리 수준**  
```
1. 트랜잭션 격리 수준(Isolation Level)
 - 트랜잭션에서 일관성이 없는 데이터를 허용하도록 하는 수준 

2. 필요성
 - 데이터베이스는 ACID(트랜잭션 특징) 같이 원자적이면서도 독립적인 수행을 하도록 한다.
 - 그래서 Locking 이라는 개념이 등장한다.
 - 하지만 무조건적인 Locking으로 동시에 수행되는 많은 트랜잭션들을 순서대로 처리하는 방식은 성능이 떨어진다.
 - 그래서 최대한 효율적인 Locking 방법이 필요하다.

3. Isolation Level 종류
 1) Read Uncommitted (레벨0) - 커밋되지 않는 읽기
  - 트랜잭션 A가 특정 컬럼 데이터를 변경하고 있을 때(커밋하지 않은 상태) 트랜잭션 B가 read하면 트랜잭션 A가 변경한 데이터를 읽어온다.
  - 커밋되지 않는 읽기는 dirty read 문제가 있다. (트랜잭션 A가 특정 컬럼 데이터를 변경하고 롤백 했을 때 발생)
  - 데이터의 일관성을 유지할 수 없다.

 2) Read Committed (레벨1) - 커밋된 읽기
  - 트랜잭션 A가 특정 컬럼 데이터를 변경하고 있을 때(커밋하지 않은 상태) 트랜잭션 B가 read하면 트랜잭션 A가 변경하기 전 데이터를 읽어온다. 
  만약 트랜잭션 A가 데이터 변경 후 커밋하게 되면 트랜잭션 B는 변경된 데이터를 읽어온다.

 3) Repeatable Read (레벨2) - 반복 가능한 읽기
  - 항상 일관성 있는 데이터 읽기를 보장하는 레벨
  - 다른 트랜잭션에서 데이터를 조작하여도 영향을 받지 않는다.

 4) Serializable (레벨3) - 직렬화 가능
  - 가장 높은 격리 수준
  - 트랜잭션이 완료될 때까지 SELECT 문장이 사용하는 모든 데이터에 Shared Lock이 걸리므로 다른 사용자는 그 영역에 해당되는 데이터에 수정 및 입력이 불가능하다.

* DB 별 default isolation

 - MYSQL : REPEATABLE READ
 : 트랜잭션이 시작되기 전(쿼리가 시작되기 전이 아닌) commit 된 결과를 참조합니다.
 
 - ORACLE : READ COMMITTED 
 : 쿼리가 시작되기 전 다른 트랜잭션에서 commit 된 결과를 참조할 수 있으며 동일 트랜잭션 상에서 동일한 쿼리문에 대해 서로 다른 결과를 조회(Phantom-Read)할 수 있습니다.

 - MSSQL : READ COMMITTED 
 : (WITH(NOLOCK) 으로 select 할 경우의 격리수준(Isolation Level)은 Read Uncommitted)
 
```

**-트랜잭션 전파 옵션**  
```
1. 전파 옵션
 1) REQUIRED
   - 기본 속성. 부모 트랜잭션 내에서 실행하며 부모 트랜잭션이 없을 경우 새로운 트랜잭션을 생성한다.

 2) SUPPORTS
   - 이미 시작된 트랜잭션이 있으면 참여하고 이외에는 트랜잭션 없이 진행하게 만든다.

 3) REQUIRES_NEW
   - 부모 트랜잭션을 무시하고 무조건 새로운 트랜잭션이 생성된다.

 4) MANDATORY
  - REQUIRED와 비슷하게 이미 시작된 트랜잭션이 있으면 참여한다.
  - 트랜잭션이 없을 경우에는 예외를 발생한다.
  - 혼자서는 독립적으로 트랜잭션을 진행하면 안되는 경우에 사용한다.

 5) NOT_SUPPORTED
  - 트랜잭션을 사용하지 않게 한다.
  - 이미 진행 중인 트랜잭션이 있으면 보류 시킨다.

 6) NAVER
  - 트랜잭션을 사용하지 않도록 강제한다.
  - 이미 진행 중인 트랜잭션도 존재하면 안되며 있을 경우 예외를 발생시킨다.

 7) NESTED
  - 이미 진행중인 트랜잭션이 있으면 중첩 트랜잭션을 시작한다.
  - 중첩된 트랜잭션은 먼저 시작된 부모 트랜잭션의 커밋과 롤백에 영향을 받지만, 자신의 커밋과 롤백은 부모 트랜잭션에 영향을 미치지 않는다.
```

<br/>

>**Q&A**  

[-트랜잭션 ACID](https://covenant.tistory.com/85)  
[-testing-stub, mock, spy 차이](https://luran.me/343)  
[상태검증 assertThat, 행위검증 verify](https://joont92.github.io/tdd/%EC%83%81%ED%83%9C%EA%B2%80%EC%A6%9D%EA%B3%BC-%ED%96%89%EC%9C%84%EA%B2%80%EC%A6%9D-stub%EA%B3%BC-mock-%EC%B0%A8%EC%9D%B4/)  
[으뜸별-테스트대역](https://beststar-1.tistory.com/29)  


>**References** 

[서비스 추상화 정리](https://xlffm3.github.io/spring%20&%20spring%20boot/toby-spring-chapter5/)   
[우아한형제들 Java Enum 활용기](https://techblog.woowahan.com/2527/)  
[이펙티브자바 item34 - int 상수 대신 열거 타입을 사용하라](https://jjingho.tistory.com/81)  
[Enum예제](https://shinysblog.tistory.com/29)  
[java11 oracle document](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/Enum.html)   
[트랜잭션 격리 수준](https://snow-line.tistory.com/145)   
[트랜잭션 전파 옵션](https://snow-line.tistory.com/146)   
[MSSQL with(nolock)](https://ryean.tistory.com/32)  
[Read committed VS Repeatable read](https://corekms.tistory.com/entry/Read-committed-VS-Repeatable-read)  
