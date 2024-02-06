# URI & MIME Type
### URI(Uniform Resource Identifier)
URI : 리소스를 식별하는 방법을 URI라고 한다.<br>
### URL과 URN
URL : URL은 리소스의 위치를 나타내는 방법으로 Uniform Resource Locator가 풀네임이며 우리가 잘 알다시피 웹 주소를 말한다.<br>
URN : URN은 리소스의 이름을 통하여 리소스를 식별하는 방법이고 Uniform Resource Name이다.<br><br>
URL과 URN 둘 다 리소스를 식별하는 방법이지만 거의 대부분 URL을 사용한다.<br> <br>
### URL과 URN의 예시
우선 URN의 예시는 다음과 같다.
> urn:isbn:9780141036144

<br>
오늘날에는 대부분 URL을 사용하므로 URL에 대해서 자세하게 알아보자.<br><br>

 ``http://www.example.com:80/path/to/myfile.html?key1=value1&key2=value2#SomewhereInTheDocument``

 http:// -> 프로토콜<br>
 www.example.com -> 도메인 네임, 여기에는 IP 주소를 직접사용할 수 있지만 편의성이 떨어져 웹에서는 자주 사용되지 않는다.<br>
 :80 -> 포트<br>
 /path/to/myfile.html -> 리소스의 경로<br>
?key1=value1&key2=value2 -> 쿼리로 웹서버에 제공되는 추가 매개변수 입니다. ?기호로 시작하고 &기호로 구분된 키=값 쌍의 목록이다.<br>
#SomewhereInTheDocument -> 프레그먼트로 일종의 북마크라고 이해하자. #뒤의 부분은 요청과 함께 서버로 전송되지 않는다.<br><br>

참고<br>
[MDN : URI](https://developer.mozilla.org/ko/docs/Web/HTTP/Basics_of_HTTP/Identifying_resources_on_the_Web)


### MIME Type (Content Type, Media Type)

MIME Type은 미디어 타입으로 문서, 파일 또는 바이트 집합의 성격과 형식을 나타낸다.<br>
브라우저는 MIME 타입을 사용하여 URL 처리 방법을 결정한다. <br>
따라서 웹서버가 응답의 Content-type 헤더에 올바른 MIME 타입을 보내는 것이 중요하다.<br> 
올바르지 않은 MIME Type을 구성하면 사이트가 제대로 작동하지 않을 가능성이 높다.<br><br>
### MIME Type의 구조
> type/subtype

MIME Type은 항상 타입과 하위타입을 가지며 "/"(슬래시)로 구분하고 둘 사이에 공백은 없다.<br><br>
### 자주 사용하는 MIME Type
- text/plain -> 텍스트 파일에 대한 기본값이며 특히 E-mail에서 자주 사용된다.<br>
- text/html -> 일반적인 html 컨텐츠에 이 타입이 사용된다.
- text/css -> css 파일은 모두 이 타입을 사용한다.
- text/javascript -> javascript 파일은 모두 이 타입을 사용한다.
- application/xml -> xml을 표현할 때 사용하지만 REST의 제약조건인 자기서술적인 메세지를 만족하지 못한다.
- application/atom+xml -> 이렇게 쓰면 자기서술적인 메세지를 만족할 수 있다.
- application/json -> json을 표현할 때 사용, 이 또한 범용적이기 때문에 자기서술적인 메세지를 만족하지 못한다.
- application/dns+json -> 자기서술적인 메세지에 만족한다.<br><br>

참고<br>
[MDN : MIME Type](https://developer.mozilla.org/ko/docs/Web/HTTP/Basics_of_HTTP/MIME_types)<br>
[IANA : Media Types](https://www.iana.org/assignments/media-types/media-types.xhtml)
