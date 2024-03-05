# CORS
###  CORS란?
CORS (Croos-Origin Resource Sharing)는 직역하면 교차 출처 리소스 공유로 이를 설명하기 앞서 Same-Origin Policy(동일 출처 정책)에 대해서 알고나면 쉽게 이해할 수 있을 것이다.<br><br>
**Same-Origin Policy**<br>
동일 출처 정책은 요청하는 쪽과 응답하는 쪽 사이에 동일한 리소스를 주고 받아야한다는 정책이다. <br>
예를 들면 브라우저에서 `http://foo.com` 이라는 리소스를 보냈다면 서버에서도 같은 `http://foo.com`으로 응답을 해야한다는 말이다.<br><br>

다시 CORS로 넘어가서 CORS는 동일 출처 정책을 위반? (어감이 좀 이상하긴 하지만...) 할 수 있는 방법이다. 그러면 CORS는 왜 사용되게 되었을까?<br><br>

A 라는 브라우저에서 B 서버에 있는 리소스를 가져와서 쓰고 싶은 데 웹 브라우저의 보안 원칙은 동일 출처 정책을 따르고 있기 때문에 요청하는 A 브라우저의 출처(Origin)과 B 서버의 출처(Origin)이 다르기 때문에 B 서버의 리소스를 가져와 쓸 수가 없다. 이러한 문제를 해결하기 위해서 CORS가 도입된 것이다.<br><br>

### 동일 출처 정책을 우회하는 방법들
**JSONP**<br>
JSONP는 클라이언트가 아닌 다른 서버의 리소스의 데이터를 가져올 수 있는 방법으로  동일 출처 정책을 우회할 수 있다.<br>
동일 출처 정책은 클라이언트가 아닌 다른 출처에서의 DOM요소는 XHR 데이터를 읽기 위한 자바스크립트 사용을 허용하지 않는다. 하지만 HTML의 `<script>` 태그는 다른 외부 출처에서 데이터를 가져오기 위한 자바스크립트 사용이 허용되는 점을 이용한 기술이다.<br><br>

아래 `http://server/posts`에는 JSON 데이터를 담고 있다고 가정해보자
```html
<script src="http://server/posts"></script>
```
위와 같이 JSON 데이터가 담긴 URL만 달랑 입력하면 에러가 발생한다. 왜냐하면 자바스크립트 엔진이 JSON과 같이 변수, 상수 등의 정의 없이 나오는 중괄호 문법을 block으로 해석하기 때문이다.<br><br>
```html
<script src="http://server/posts?callback=success"></script>
```
sucess라는 이름의 콜백함수를 지정해주면 웹 브라우저는 유효한 자바스크립트로 받아들이고 success라는 함수를 이용하여 JSON 데이터를 가져올 수 있다.<br>
그런데 안타깝게도 이러한 JSONP는 상속 비보안 문제로 CORS에 대체되었다.<br><br>
**CORS**<br>
서버 쪽(Back-end), REST API에 Access-Control-Aloow-Origin 헤더를 넣어 주면 CORS를 사용할 수가 있다. 서버 쪽에서 이 Access-Control-Allow-Origin 헤더에 정의된 클라이언트(Front-end)의 요청이면 허용한다라고 알려주는 방식이다.<br><br>

참고<br>
[교차 출처 공유(CORS) : MDN](https://developer.mozilla.org/ko/docs/Web/HTTP/CORS)<br>
[Access-Control-Allow-Origin](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Access-Control-Allow-Origin)
