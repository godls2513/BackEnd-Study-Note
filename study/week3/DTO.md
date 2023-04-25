## [DTO (Data Transfer Object)](https://martinfowler.com/eaaCatalog/dataTransferObject.html)
메서드 호출을 줄이기 위해 프로세스 간 데이터를 전달하는 객체<br>
getter, setter로만 이뤄짐.<br>
JavaBeans의 **Java Bean**(Bean)의 형태에 가까움(**Spring Bean, POJO와 다름!**)<br>
무기력한 데이터 덩어리

Remote Facade와 같은 원격 인터페이스는 호출 비용이 많이 들기 때문에 많은 매개변수를 이용하여 해결 할 수도 있다.<br>
&nbsp; 하지만 프로그래밍이 어렵고 Java에서는 불가능하다. (Java는 단일 값만 반환하기 때문)<vr>

**해결책**<br>
&nbsp; 호출에 대한 모든 데이터를 담을 수 있는 DTO(데이터 전송 객체)를 만들면 된다.<br>
&nbsp; DTO를 이용하여 연결을 통한 전송을 하기 위해서는 직렬화(마샬링)이 필요하다.
<br><br>
DTO를 사용하는 주된 이유<br>
1. 여러 원격 호출을 단일로 처리하기 위해
2. 직렬화를 캡슐화

### Remote Facade
작은 메서드를 가진 작은 객체를 사용하여 많은 메서드 호출을 필요하게 한다<br>

- 작은 메서드를 가진 작은 객체
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
  + REST를 활용하면 RPC(SOAP의 일반적인 활용)가 아닌 Resource에 대한 CRUD로 정리해야 함.
- Java에선 RMI(Remote Method Invocation)란 기술을 제공한다.
  + [RPC(Remote Process Call)](https://ko.wikipedia.org/wiki/%EC%9B%90%EA%B2%A9_%ED%94%84%EB%A1%9C%EC%8B%9C%EC%A0%80_%ED%98%B8%EC%B6%9C)<br> 
    - 별도의 원격제어를 위한 코딩없이 다른 주소에 있는 함수나 프로시저를 실행할 수 있게하는 프로세스 간의 통신기술
  + [RMI(Remote Method Invocation)](https://ko.wikipedia.org/wiki/%EC%9E%90%EB%B0%94_%EC%9B%90%EA%B2%A9_%ED%95%A8%EC%88%98_%ED%98%B8%EC%B6%9C)<br>
    + 자바에서 각 객체간, 컴퓨터간 메서드를 호출할 수 있게 해주는 기술

REST에서는 표현을 다뤄야하고, 데이터를 담는 거 외에 아무 것도 하지 않은 제대로 된 객체라고 볼 수 없는 특별한 객체를 사용하게 된다.

---

**Tier간 통신**
<br>
- 프론트와 백엔드 사이
  + DTO는 직렬화(마샬링)을 통해서 전송할 수 있다.
  + 직렬화를 하는 기술의 선택이 필요하다. &rarr; XML, JSON
- 백엔드와 DB사이
  + VO(Value Object) 구별하여 Transfer Object로 정정. 아직도 SI에서는 VO와 DTO를 비슷한 의미로 사용. (DAO와 VO를 같이 사용한다면 대부분 여기에 속함).











