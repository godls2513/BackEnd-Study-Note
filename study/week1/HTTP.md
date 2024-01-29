## HTTP의 이해
---

### HTTP 
HTML 문서와 같은 리소스들을 가져올 수 있도록 해주는 프로토콜
<br>

### 프로토콜
&nbsp; 컴퓨터 사이에서 데이터 교환방식을 정의하는 규칙 체계
### 클라이언트 - 서버 모델
 서비스 요청자인 클라이언트와 서비스 자원의 제공자인 서버 간에 작업을 분리해주는 분산 애플리케이션 구조이자 네트워크 아키텍처를 나타낸다<br>

 ![alt text](https://upload.wikimedia.org/wikipedia/commons/thumb/c/c9/Client-server-model.svg/500px-Client-server-model.svg.png)
<br><br>
## [stateless, stateful](https://en.wikipedia.org/wiki/Stateless_protocol)
### stateful(무상태)
&nbsp; - HTTP는 상태를 저장하지 않는다. <br>
&nbsp; 즉, 수신자가 이전 요청의 세션 상태를 보유하지 않아야 하는 상태를 말한다.<br>
&nbsp; - 각각의 요청이 독립적이다.<br>
&nbsp; - 클라이언트는 항상 자신이 누구인지 알려줘야한다. 이것이 성능저하의 문제가 될 수 있다.<br>
&nbsp; - HTTP Cookie로 세션을 만들어 해결

### stateful(상태유지)
&nbsp; - 이전 요청의 세션 상태를 유지할 수 있는 특성<br>
&nbsp; - TCP(전송 제어 프로토콜) 및 FTP(파일 전송 프로토콜)가 여기에 해당<br>

### HTTP 프로토콜에서 상태정보를 기억시켜주는 [HTTP Cookie](https://developer.mozilla.org/ko/docs/Web/HTTP/Cookies)
&nbsp; - 사용자가 웹사이트를 탐색하는 동안 웹 서버에 의해 생성되고 사용자의 웹 브라우저에 의해 사용자의 컴퓨터 또는 기타 장치에 저장되는 작은 데이터 블록,조각<br>
&nbsp; - 브라우저는 데이터 조각들을 저장했다가 동일한 요청이 들어오면 저장된 쿠키를 사용한다.<br>
&nbsp; - 두 요청이 동일한 브라우저에서 들어왔는지 판단할 때 사용<br>

&nbsp; ex) 장바구니 저장, 로그인 상태 유지, 과거 방문 페이지 기록

### [HTTP Session](https://developer.mozilla.org/ko/docs/Web/HTTP/Session) 

**세션의 과정**<br>
1. 클라이언트가 TCP 연결을 수립
2. 클라이언트는 요청을 전송한 뒤 응답을 기다림
3. 서버는 요청에 대해 처리하고 그에 대한 응답을 상태 코드, 요청에 부합하는 데이터와 함께 돌려보냄

### HTTP 요청 메서드
&nbsp; - 주어진 리소스에 수행하기 원하는 행동, 동작<br>
&nbsp; - 일반적으로 대부분의 요청은 GET, POST<br>
GET &rarr; read<br>
[HEAD](https://developer.mozilla.org/ko/docs/Web/HTTP/Methods/HEAD) : GET과 동일한 응답을 요구하지만 응답 본문은 포함하지 않음<br>
[POST](https://developer.mozilla.org/ko/docs/Web/HTTP/Methods/POST)<br>
&nbsp; - 서버로 데이터를 전송, 멱등성을 가지지 않음<br>
&nbsp; - [멱등성](https://developer.mozilla.org/ko/docs/Glossary/Idempotent) : 동일한 요청을 한번 보내는 것과 여러번 연속으로 보내는 것이 똑같은 효과를 지니고, 서버의 상태가 동일하게 남는 특성<br>
[PUT](https://developer.mozilla.org/ko/docs/Web/HTTP/Methods/PUT)<br>
&nbsp; - 요청 페이로드를 사용해 새로운 리소스를 생성하거나, 대상 리소스를 나타내는 데이터를 대체<br>
&nbsp; - 한 번을 보내도, 여러 번을 연속으로 보내도 같은 효과를 보임, 멱등성을 가짐<br>
&nbsp; - [페이로드](https://ko.wikipedia.org/wiki/%ED%8E%98%EC%9D%B4%EB%A1%9C%EB%93%9C_(%EC%BB%B4%ED%93%A8%ED%8C%85)_) : 사용에 있어서 전송되는 데이터<br>
[PATCH](https://developer.mozilla.org/ko/docs/Web/HTTP/Methods/PATCH) : 리소스의 부분적인 수정을 할 때 사용, 멱등성을 가지지 않음<br>
[DELETE](https://developer.mozilla.org/ko/docs/Web/HTTP/Methods/DELETE) : 지정한 리소스를 삭제<br>
[OPTION](https://developer.mozilla.org/ko/docs/Web/HTTP/Methods/OPTIONS) : 주어진 URL 또는 서버에 대해 허용된 통신 옵션을 요청<br>

### [HTTP Status Code](https://developer.mozilla.org/ko/docs/Web/HTTP/Status)
HTTP 요청이 성공적으로 완료되었는지 알려주는 상태코드<br>
1xx : 정보 응답 &rarr; 우리가 직접 사용하는 일은 드뭄<br>
2xx : 성공 응답 &Rightarrow; 200 OK, 201 Created, 204 No Content<br>
3xx : 리다이렉션 메세지<br>
4xx : 클라이언트 에러 응답<br>
5xx : 서버 에러 응답<br>