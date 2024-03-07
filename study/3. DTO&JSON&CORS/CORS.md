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
[Access-Control-Allow-Origin](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Access-Control-Allow-Origin)<br><br>

### Spring MVC에서 CORS 사용하는 방법

우선 `https://test.com` 이라는 프론트 페이지가 있다고 가정을 하고 이 페이지에서 백엔드 API 서버인 `http://localhost:8080/posts`에 요청을 보내는 시나리오이다.<br>
```java
@RestController
@RequestMapping("/posts")
public class PostController {
    private final ObjectMapper objectMapper;

    public PostController(ObjectMapper objectMapper) {
        this.objectMapper = objectMapper;
    }

    List<PostDto> postDtos = new ArrayList<>(List.of(
            new PostDto("123", "제목", "테스트입니다."),
            new PostDto("456", "제목2", "2번테스트입니다.")
    ));

    @GetMapping
    public List<PostDto> list() {
        return postDtos;
    }
}
```

프론트 페이지인 `https://test.com` 에서 백엔드 API 서버인 `http://localhost:8080/posts` 에 GET 요청을 보내면 다음과 같은 에러를 확인할 수 있다.<br>
![](/study/3.%20DTO&JSON&CORS/images/CORS%20Error.jpg)<br>
위 그림은 개발자 도구에의 네트워크 탭에서 확인한 내용으로 주목할 부분은 **"CORS error"** 이다.<br><br>
요청하는 쪽인 프론트 페이지와 응답하는 쪽인 백엔드 API서버 사이에 CORS 즉 Cross Origin Resource Sharing 을 하지 못해서 제대로된 응답을 받지 못하는 것이다. 위 소스 코드를 CORS를 허용하도록 하려면 다음과 같이 코드를 작성할 수 있다.<br>
```java
    @GetMapping
    public List<PostDto> list(HttpServletResonse response) {
        response.addHeader("Access-Control-Allow-Origin","https://test.com")
        return postDtos;
    }
```
HTTP 헤더에 Access-Control-Allow-Origin 을 추가하고 값으로 요청하는 URL을 넣으면 CORS error 문제를 해결할 수가 있다. Access-Control-Allow-Origin을 쉽게 설명하자면 서버에서 `https://test.com`으로 온 요청은 응답을 주겠다고 허용하는 것이다.<br>
만약 백엔드 서버가 모든 요청을 허용하고 싶다면 `*`(와일드카드)만 사용하면 된다<br><br>
`response.addHeader("Access-Control-Allow-Origin","*")`
<br><br>

위 코드도 깔끔하지만 스프링은 CORS에 대한 더 대단한 어노테이션을 제공하는데 바로 `@CrossOrigin` 이다.
```java
    @GetMapping
    @CrossOrigin("https://test.com")
    public List<PostDto> list() {
        return postDtos;
    }
```
메서드 위에 @CrossOrigin에 CORS를 적용할 프론트 URL만 입력하면 끝이다.<br><br>
`@CrossOrigin` 은 메서드 레벨에서 사용할 수도 있고 아예 클래스 전체에 사용할 수도 있다. 상황에 맞게 사용을 하면 된다.<br>
`@CrossOrgin` 뒤에 아무것도 쓰지 않으면 `*`와 같은 기능을 한다.<br><br>
다음은 HttpServletResponse를 이용하여 CORS를 적용한 다음 서버에 요청을 하면 다음과 같은 응답을 받을 수가 있다.<br>
![](/study/3.%20DTO&JSON&CORS/images/preflight.jpg)<br>
200, 요청에 대한 응답을 성공적으로 수행했는데 같은 posts가 403 상태코드와 preflight를 보여주고 있는데 CORS는 프론트에서 요청을 OPTIONS로 'preflight'(미리 전달)하여 지원하는 메서드를 요청하고, 서버의 허가가 떨어지면 실제 요청을 보내도록 요구한다.<br> 즉, preflight 요청은 OPTIONS로 사전에 어떤 HTTP 메서드를 허용할지 정의할 수가 있다.<br><br>
preflight는 아래와 같이 정의할 수 있다.

```java
    @Bean
    public WebMvcConfigurer webMvcConfigurer() {
        return new WebMvcConfigurer() {

            @Override
            public void addCorsMappings(CorsRegistry registry) {
                registry.addMapping("/**")
                        .allowedMethods("GET", "POST", "PATCH", "DELETE", "OPTIONS")
                        .allowedOrigins("https://test.com");
            }
        };
    }
```
WebMvcConfigurer 라는 스프링에서 제공하는 인터페이스에 addCorsMappings()이라는 메서드가 존재하는데 이 메서드를 통해 CORS를 전역 단위로 설정해줄 수가 있다.(메서드 레벨이나 클래스 레벨에 하나씩 @CrossOrigin을 붙이지 않고 전역, 전체에 대해 CORS를 적용할 수 있다.)<br><br>
`.addMapping("/**")` : 적용대상은 전역(전체)<br>
 `.allowedMethods("GET", "POST", "PATCH", "DELETE", "OPTIONS")` : 위 HTTP 메서드를 허용<br>
`.allowedOrigins("https://test.com");` : 서버에서 허용하는 요청 URL <br><br>

