# REST API

## API(Aplication Programming Interface)
<br>
1. API란?<br>
API(Aplication Programming Interface)는 컴퓨터나 컴퓨터 프로그램 사이의 연결이다. 또는 소프트웨어와 다른 소프트웨어 사이의 연결하는 일종의 인터페이스이다.<br><br>
2. API의 목적<br>
API는 다른 소프트웨어의 기능을 쉽게 사용할 수 있도록 하는 인터페이스로 내부의 세세한 내용을 몰라도 프로그래머가 API의 기능을 사용할 수 있도록 한다. 이는 밑에서 이야기할 '정보은닉' 과  관련이 있다.<br><br>

## 정보은닉과 캡슐화<br>
정보은닉(Information Hiding)은 변경될 가능성이 있는 컴포넌트의 설계결정을 해당 컴포넌트에 의존하는 다른 컴포넌트로부터 숨기는 **원칙**이다. 예를 들면 리모컨을 사용할 때 우리는 사용방법만 알면 되지 리모컨 내부가 어떻게 만들어져 있는지는 알 필요가 없는 것이 정보은닉의 예로 들 수 있다.<br><br>

캡슐화(Incapsulation)는 관련항목을 그룹화하거나 함께 묶어 복잡성을 숨기고 정보에 대한 엑세스 제어를 하는 **기술**이다.
예를들면 자바 클래스에서 자동차 부품을 Car클래스로 묶는 예를 들 수가 있다.<br>
[참고: medium.com](https://tan-jun-bang.medium.com/what-is-information-hiding-principle-c5d5b97d4595)<br><br>
## Architecture 와 Architecture style 의 차이<br>
아키텍처란 ***서비스의 동작 원리***를 나타내는 것으로 하나의 서비스가 어떻게 구성되며 어떻게 동작이 하는지, 무엇과 상호작용을 하는지, 정보가 어떻게 교환되는지를 표현하는 것이다. <br>
>참고<br>
[www.osckorea.com](https://www.osckorea.com/post/bigaebaljado-swibge-ihaehaneun-akitegceoyi-gaenyeom)<br>
[https://brunch.co.kr/@taehyo/7](https://brunch.co.kr/@taehyo/7)<br>
[wikipedia](https://ko.wikipedia.org/wiki/%EC%8B%9C%EC%8A%A4%ED%85%9C_%EC%95%84%ED%82%A4%ED%85%8D%EC%B2%98)<br>
[Software Architecture(wikipedia)](https://en.wikipedia.org/wiki/Software_architecture)<br>

반면에 아키텍처 스타일이란 그 스타일을 따르는 제약조건들의 집합이고 시스템의 구성을 안내하는 일련의 원칙과 패턴을 말한다.
<br>
참고<br>
[Linkedin](https://www.linkedin.com/pulse/understanding-software-architecture-design-styles-chapter-michael/)<br><br>

## REST<br>
REST는 REpresentational State Transfer의 줄임말로 웹을 위한 아키텍처 스타일이며, REST는 웹과 같은 분산 하이퍼미디어 시스템의 아키텍처가 어떻게 작동해야 하는지에 대한 일련의 제약조건을 정의한다. <br>
추가로 REST라는 말을 처음 사용한 사람은 Roy Fielding 이다.<br><br>
REST의 원칙은 다음과 같은 제약조건이 필요하다.
- Client - Server Architecture
- Statelessness
- Cacheability
- Uniform interface
- Code on Demand(Optianal)<br>

각 제약조건의 설명은 다음과 같다.<br>
Client - Server Aritecture<br>
클라이언트-서버 아키텍처는 사용자와 데이터 저장을 분리하는 원칙을 적용함으로써 사용자 인터페이스의 이식성이 향상되고 이러한 분리는 서버 구성요소를 단순화하여 확장성을 향상시켜 인터넷에 필요한 구성요소가 독립적으로 진화할 수 있다.

Statelessness (무상태)<br>
무상태는 서버(수신자)가 세션정보를 보유하지 않은 특징이다. 따라서 서버의 부하를 제거하여 성능을 향상시킨다.

Cacheability<br>
클라이언트의 추가 요청이 있을 때 응답으로 오래되거나 부적절한 데이터의 전송을 방지하기 위해 캐시가 가능하다. 이는 클라이언트와 서버간의 상호작용을 제거하여 확장성과 성능을 향상 시킨다.

Layered system<br>
계층화를 하면 시스템의 확장성을 향상시킬 수가 있다. 각 레이어에 속한 구성요소는 인접하지 않은 레이어의 구성요소를 볼 수 없어야한다.

Code on demand (optional)<br>
서버는 실행코드를 전송하여 클라이언트의 기능을 일시적으로 확장하거나 사용자가 지정할 수 있다. (예를 들면 javascript)

**Uniform Interface**<br>
균일한 인터페이스는 아키텍처를 단순화하고 분리하여 각 부분이 ***독립적으로 발전***할 수 있도록 한다. uniform interface를 지키려면 다음 4가지 제약조건이 필요하다.<br>
1. Resource of Identification : URI 등으로 리소스를 식별한다.<br>
2. Manipulation of Resource through Representation : 표현으로 리소스를 조작한다.<br>
3. Self-descriptive Messages : 메세지는 자기 서술적이어야한다.<br>
4. Hypermedia as the Engine of Application State : HATEOAS 라고 부르기도 하는데 하이퍼미디어 링크가 포함되어 있어야한다.<br>

참고<br>
[blog.npcode.com : 바쁜 개발자들을 위한 REST 논문 요약](https://blog.npcode.com/2017/03/02/%eb%b0%94%ec%81%9c-%ea%b0%9c%eb%b0%9c%ec%9e%90%eb%93%a4%ec%9d%84-%ec%9c%84%ed%95%9c-rest-%eb%85%bc%eb%ac%b8-%ec%9a%94%ec%95%bd/)<br>
[위키피디아 : RESTful](https://en.wikipedia.org/wiki/REST)<br>
[리차드슨의 성숙도모델](https://martinfowler.com/articles/richardsonMaturityModel.html)<br>
