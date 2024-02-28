# 스프링에서 Jackson ObjectMapper 사용하기
Jackson ObjectMapper는 자바객체를 JSON 데이터로 직렬화 하거나 JSON 데이터를 자바객체로 역질력화하는 Jackson 라이브러리의 클래스이다.<br><br>
스프링은 Jackson 라이브러리가 내장되어 있어 ObjectMapper를 별도의 설치없이 사용할 수가 있다.
이번 장에서는 스프링에서는 어떻게 Jackson ObjectMapper를 사용하는지 알아보자.<br> 

## DTO 생성
우선 데이터를 담는 객체에 불과한 DTO를 생성해준다.<br>
```java
public class PostDto {
    private String id;
    private String title;
    private String content;

    public PostDto() {
    }

    public PostDto(String id, String title, String content) {
        this.id = id;
        this.title = title;
        this.content = content;
    }
  // getter, setter 추가
}
```
DTO에 대해 알고 싶다면 [여기](/study/3.%20DTO&JSON&CORS/DTO.md)를 참고하시오.<br><br>
## 컨트롤러 생성
DTO에서 데이터를 받아서 사용하기 위한 컨트롤러 클래스를 다음과 같이 작성해준다.<br>
```java
@RestController
@RequestMapping("/jackson_posts")
public class JacksonPostController {

    private final ObjectMapper objectMapper;

    public JacksonPostController(ObjectMapper objectMapper) {
        this.objectMapper = objectMapper;
    }

    ArrayList<PostDto> postDtos = new ArrayList<>(List.of(
            new PostDto("123", "제목", "테스트입니다."),
            new PostDto("456", "제목2", "2번테스트입니다.")
    ));

    @GetMapping("/jackson/{id}")
    public String detail(@PathVariable("id") String id) throws JacksonException {
        PostDto postDto = new PostDto(id, "제목", "내용이다.");

        return objectMapper.writeValueAsString(postDto);
    }

    @GetMapping("/{id}")
    public PostDto detail2(@PathVariable("id") String id) {
        PostDto postDto = new PostDto(id, "제목", "테스트입니다.");
        return postDto;
    }
}
```
### 해석
```java
    private final ObjectMapper objectMapper;
```
ObjectMapper를 final로 선언해주면서 스프링 DI를 통해 컨트롤러에서 Jackson ObjectMapper를 얻을 수 있다. final로 선언했으니까 생성자도 함께 생성한다.<br>

```java
    public JacksonPostController(ObjectMapper objectMapper) {
        this.objectMapper = objectMapper;
    }
```
다음 코드는 게시판 API의 일부인데, 게시판의 상세내용을 보여주는 API이다.
```java
    @GetMapping("/jackson/{id}")
    public String detail(@PathVariable("id") String id) throws JacksonException {
        PostDto postDto = new PostDto(id, "제목", "내용이다.");

        return objectMapper.writeValueAsString(postDto);
    }
```

PostDto 객체의 인자에 데이터를 각각 id , "제목", "내용이다." 로 저장하는 객체를 생성하고 바로 제일 위에서 선언했던 objectMapper를 통해 objectMapper.writeValueAsString(postDto)를 반환한다.<br>
writeValueAsString()은 자바객체를 JSON 데이터로 직렬화하는 메서드이다.<br><br>
**출력결과**
![](/study/3.%20DTO&JSON&CORS/images/JacksonDetail.jpg)

스프링에는 Jackson 라이브러리가 내장되어있어 ObjectMapper를 사용할 수가 있다고한 것을 기억하는가? 스프링에서는 메서드를 통해 반환할 필요없이 자동으로 JSON 데이터로 직렬화할 수도 있다.<br>

```java
    @GetMapping("/{id}")
    public PostDto detail2(@PathVariable("id") String id) {
        PostDto postDto = new PostDto(id, "제목", "테스트입니다.");
        return postDto;
    }
```
detail2 메서드는 PostDto를 반환타입으로 하는 메서드이며 생성된 객체를 바로 리턴해주면 자동으로 스프링이 JSON 데이터로 변환해준다.<br><br>
**출력결과**
![](/study/3.%20DTO&JSON&CORS/images/JacksonDetail2.jpg)
