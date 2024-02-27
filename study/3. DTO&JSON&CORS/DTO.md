# DTO 

###  [DTO (Data Transfer Object)](https://martinfowler.com/eaaCatalog/dataTransferObject.html)

- 메서드 호출을 줄이기 위해 프로세스 간 데이터를 전달하는 객체<br>
- getter, setter로만 이뤄짐.<br>
- JavaBeans의 **Java Bean**(Bean)의 형태에 가까움(**Spring Bean, POJO와 다름!**)<br>

### DTO를 사용하는 이유
Remote Facade와 같은 원격 인터페이스는 호출 비용이 많이 든다. 프로그램의 성능을 위해서는 결과적으로 이러한 호출 횟수를 줄여야 한다. 이를 해결하기 위해서는 많은 매개변수를 이용하면 된다. 하지만 프로그래밍이 어렵고 Java에서는 불가능하다. (Java는 단일 값만 반환하기 때문)<br>

**해결책**<br>
&nbsp; 호출에 대한 모든 데이터를 담을 수 있는 DTO(데이터 전송 객체)를 만들면 된다.<br>
&nbsp; DTO를 이용하여 연결을 통한 전송을 하기 위해서는 직렬화(마샬링)이 필요하다.
<br><br>

**DTO를 사용하는 주된 이유**<br>
1. 여러 원격 호출을 일괄로 처리하기 할 수 있다.
2. 데이터 전송을 위한 직렬화 매커니즘을 캡슐화할 수 있다.

### Remote Facade
작은 메서드를 가진 작은 객체를 사용하여 많은 메서드 호출을 필요하게 한다<br>

&nbsp; 작은 메서드를 가진 작은 객체<br>

> 동작을 제어하여 대체하고, 의도가 잘 드러난 네이밍으로 애플리케이션을 쉽게 이해하는 객체로 객체간 많은 상호작용이 발생하고 이러한 상호 작용에는 많은 메서드가 필요하다.

---
<br>
IPC(Inter-Process Communication) 
<br><br>
&nbsp; - 서로 다른 프로세스, 프로그램 간에 통신<br>
&nbsp; - 백엔드와 프론트엔드로 Tier(계층)를 나누면 IPC가 필수적이다.<br>
&nbsp; - IPC에서 쓸 수 있는 기술<br>

- File &rarr; 가장 기본 적인 접근, 원격 환경에서 활용하기 어렵다.
- Socket &rarr; 파일과 유사하게 읽고 쓸 수 있지만, 원격 환경에서도 활용할 수 있다.
  + HTTP 같은 고수준 프로토콜을 활용하면 어느 정도 정해진 틀이 있기 때문에 상대적으로 쉬워진다.
  + **REST**를 활용하면 RPC(SOAP의 일반적인 활용)가 아닌 **Resource**에 대한 **CRUD**로 정리해야 함.
- Java에선 RMI(Remote Method Invocation)란 기술을 제공한다.

[RPC(Remote Process Call)](https://ko.wikipedia.org/wiki/%EC%9B%90%EA%B2%A9_%ED%94%84%EB%A1%9C%EC%8B%9C%EC%A0%80_%ED%98%B8%EC%B6%9C)<br> 
  - 별도의 원격제어를 위한 코딩없이 다른 주소에 있는 함수나 프로시저를 실행할 수 있게하는 프로세스 간의 통신기술

[RMI(Remote Method Invocation)](https://ko.wikipedia.org/wiki/%EC%9E%90%EB%B0%94_%EC%9B%90%EA%B2%A9_%ED%95%A8%EC%88%98_%ED%98%B8%EC%B6%9C)<br>
  - 자바에서 각 객체간, 컴퓨터간 메서드를 호출할 수 있게 해주는 기술

REST에서는 표현을 다뤄야하고, 데이터를 담는 거 외에 아무 것도 하지 않은 제대로 된 객체라고 볼 수 없는 특별한 객체(**DTO**)를 사용하게 된다.<br><br>
SOAP : SOAP(Simple Object Access Protocol)은 일반적으로 널리 알려진 HTTP, HTTPS, SMTP 등을 통해 XML 기반의 메시지를 컴퓨터 네트워크 상에서 교환하는 프로토콜이다.<br> 

[무기력한 도메인 모델](https://martinfowler.com/bliki/AnemicDomainModel.html)<br>

무기력한 도메인 모델은 마틴 파울러가 안티패턴으로 지정한 모델로 아무런 로직을 수행하지 않은 모델로 그냥 데이터 덩어리일뿐이다. 이는 객체지향 설계 원칙을 준수하지 못하여 유지보수성과 확장성을 어렵게 한다.

---

**Tier간 통신**
<br>
- 프론트와 백엔드 사이
  + DTO는 [직렬화(마샬링)](/study/3.%20DTO&JSON&CORS/Serialization.md)을 통해서 전송할 수 있다.
  + 직렬화를 하는 기술의 선택이 필요하다. &rarr; XML, JSON<br><br>
- 백엔드와 DB사이
  + VO(Value Object) 구별하여 Transfer Object로 정정. 아직도 SI에서는 VO와 DTO를 비슷한 의미로 사용. (DAO와 VO를 같이 사용한다면 대부분 여기에 속함).
---

### 자바빈즈(JavaBeans)
자바빈즈는 자바로 작성된 소프트웨어 컴포넌트로 어디서든 재사용이 가능한 특징이 있다.<br><br>
자바빈즈를 사용하려면 몇가지 제약조건이 따른다.<br>
- public 기본 생성자: 프레임워크 또는 애플리케이션이 Java Bean을 인스턴스화할 수 있도록 하려면 public 기본 생성자가 있어야 한다.
- Public으로 접근할 수 있는 Private Field: Java Bean 내의 캡슐화된 필드는 데이터에 액세스하고 수정할 수 있는 공용 Getter 및 Setter 메서드와 함께 Private으로 선언해야 한다.
- Serializable: Java Bean은 상태를 저장하고 복원할 수 있는 Serializable 인터페이스를 구현해야 한다. 즉 **직렬화**를 할 수 있어야한다.
- 이벤트 지원: Java Bean은 적절한 리스너 인터페이스를 구현하여 이벤트를 지원함으로써 다른 컴포넌트가 상태 변경에 대한 알림을 받을 수 있도록 합니다.

자바빈즈 참고<br>
[Understanding Java Beans: A Comprehensive Guide for Beginners](https://medium.com/@mgm06bm/understanding-java-beans-a-comprehensive-guide-for-beginners-684163011c82#id_token=eyJhbGciOiJSUzI1NiIsImtpZCI6IjU1YzE4OGE4MzU0NmZjMTg4ZTUxNTc2YmE3MjgzNmUwNjAwZThiNzMiLCJ0eXAiOiJKV1QifQ.eyJpc3MiOiJodHRwczovL2FjY291bnRzLmdvb2dsZS5jb20iLCJhenAiOiIyMTYyOTYwMzU4MzQtazFrNnFlMDYwczJ0cDJhMmphbTRsamRjbXMwMHN0dGcuYXBwcy5nb29nbGV1c2VyY29udGVudC5jb20iLCJhdWQiOiIyMTYyOTYwMzU4MzQtazFrNnFlMDYwczJ0cDJhMmphbTRsamRjbXMwMHN0dGcuYXBwcy5nb29nbGV1c2VyY29udGVudC5jb20iLCJzdWIiOiIxMTY2NjE1Mjg0OTE0MDM0Nzk5MjIiLCJlbWFpbCI6ImdvZGxzMjMwMEBnbWFpbC5jb20iLCJlbWFpbF92ZXJpZmllZCI6dHJ1ZSwibmJmIjoxNzA4NDM1MzM0LCJuYW1lIjoi7ZW07J24IiwicGljdHVyZSI6Imh0dHBzOi8vbGgzLmdvb2dsZXVzZXJjb250ZW50LmNvbS9hL0FDZzhvY0lmMklxWU15ZWpWUnFEYWRzT2dqOE1VYl9yR1RrdXU0U3NfYTFqbmhSWHJwTT1zOTYtYyIsImdpdmVuX25hbWUiOiLtlbTsnbgiLCJsb2NhbGUiOiJrbyIsImlhdCI6MTcwODQzNTYzNCwiZXhwIjoxNzA4NDM5MjM0LCJqdGkiOiIxMGMwZjEwZGM2MmI3ODc4Y2JhZjI2NzhmZmUzNTY2YmNmM2M1YWVkIn0.mNdDG_reBBCvsQyZxWzlnT4SmKWuKq3WQtDXKuMa6NwKpYS8fgFizKLMudIHP1ZXXMF4kusXt9VWwWaAsMiSYG2ikpcbPL6L9SVqLqDjCGmA8hswXNkEjN0vVRq0wyTynggRwc1_DSnVGfzrjXF8kNNNCuY7wCt1gJL5Y4pPv2t3rR1JlqdhM-96k2Wx9y9YNJC_ajaTMbcIX8ma6uCAud2njJjruqTcZq7B5_28PcJ7cICCdsGEKAHFEfSNRiWoT6nzY_Kr9gWxT-B-PtA7bhi2hN2UpqZB6Zb8rqbcixVZ63bB-rwiN2tJWsxPGV3VUfDx_b6cWgbEC8CMLpokbQ)<br>
[자바빈즈 : 위키](https://ko.wikipedia.org/wiki/%EC%9E%90%EB%B0%94%EB%B9%88%EC%A6%88)<br><br>
**EJB (Enterprise JavaBeans)**<br>
EJB는 엔터프라이즈 환경 즉, 기업환경의 시스템을 구현하기 위한 서버측 컴포넌트 모델이다. EJB는 애플리케이션의 비즈니스 로직을 가지고 있는 서버 애플리케이션이다. EJB의 사양은 Java EE의 JAVA API 중 하나로, 주로 웹 시스템에서 JSP는 화면 로직을, EJB는 비즈니스 로직을 처리한다.<br><br>
### Java record
record는 자바14에서 preview 기능으로 선보인 클래스로 상용구 코드를 줄이기 위한 특수한 유형의 클래스이다. record는 data carrier 클래스 즉 데이터를 담고 모듈간 데이터 전달을 하는 것이 목적인 클래스를 빠르게 생성하기 위해 도입되었다.<br>
위에서 언급했던 DTO라고 봐도 무방하다.<br><br>
다음은 record의 간단한 예시이다.<br>

```java
record footBallTeam (String name, int rank) {}

footBallTeam team = new fooBallTeam("Liverpool FC", 1);

System.out.println(team.name());
System.out.println(team.rank());
// 결과 : liverpool FC
          1
```
전통적인 데이터 전송 방식으로는 getter를 만들어 줄 빈 클래스를 생성하고 또 toString(), equals(), hashCode() 메서드 등을 새로 작성을 해주어야 했지만 record의 등장으로 자동으로 사용할 수가 있게 되었다. 하지만 이러한 초간단한 record에도 제한이 있습니다..<br><br>
**제한**<br>
- record는 불변 객체이기 때문에 setter를 사용할 수가 없다.
- record는 상속을 지원하지 않는다. java.lang.record가 아닌 다른클래스를 확장할 수가 없다.

참고<BR>
[Java Record Keyword: Simplify Immutable Data Carriers](https://ioflood.com/blog/java-record/)<br>
[Java record](https://jenkov.com/tutorials/java/record.html)<br>

### DAO 
DAO는 Data Access Object의 약자입니다. DAO는 데이터 지속성 로직(일반적으로 데이터베이스)을 별도의 레이어에 분리하여 사용합니다. 이렇게 하면 데이터베이스에 접근하는 저수준의 작업이 어떻게 수행되는지 알 수가 없습니다. 이를 Principle of Seperation of Logic(논리 분리 원칙)이라고도 합니다.<br><br>
참고<br>
[DAO Design Pattern](https://www.digitalocean.com/community/tutorials/dao-design-pattern)<br>
[The DAO Pattern in Java](https://www.baeldung.com/java-dao-pattern)<br><br>

### ORM

ORM은 Object Relational Mapping의 약자로 관계형 데이터베이스(Relational Database)와 객체지향 프로그래밍(OOP) 언어 사이에 데이터를 변환하는 프로그래밍 기법이다.<br><br>
관계형 데이터베이스는 SQL이라는 언어로 데이터를 표현한다. SQL은 객체지향 프로그래밍 언어와 호환이 잘 되지 않기 때문에 두 언어 사이의 데이터 교환이 하기 위해서는 ORM과 같은 도구가 필요한 것이다.<br><br>
다음은 USERS라는 테이블에 대한 SQL 쿼리문이다.
```
SELECT name, address, age, phone_number 
FROM USERS
WHERE NAME = 'BREAD' 
```
위 SQL 쿼리문은 USERS 테이블에서 NAME이 'BREAD'인 사람의 정보를 출력한다. 간단한 쿼리문이긴 하지만 ORM 도구를 사용하면 훨씬 더 간편하게 데이터를 가져올 수 있다.<br>
```
users.getByName('BREAD');
```
모든 ORM 도구에서 다 똑같은 메서드를 사용하는 건 아니지만 이러한 형식을 사용한다고 보면 된다. 이처럼 ORM은 SQL을 직접 사용하는 것보다 훨신 간편하게 데이터를 다룰 수 있게 된다.<br><br>
하지만 단점도 존재한다. 복잡한 쿼리를 다뤄야할 때는 ORM 설계가 어렵고 성능문제에 직면할 수도 있다. 











