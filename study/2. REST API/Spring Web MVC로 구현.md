## REST API를 간단한 Spring Web MVC에 녹여보기

HTTP Method와 Collection Pattern을 가지고 간단한 Spring Web MVC 프로젝트를 만들어 보자.<br><br>

핵심은 **하나의 컨트롤러**로 **하나의 리소스 Collection**을 표현하고, 리소스를 잘 **식별**하는 것이 핵심이다.<br>

```java
@RestController
@RequestMapping("/posts")
public class PostController {
    // 게시물 목록
    @GetMapping("/")
    public String list() {
        return "게시물 목록\n";
    }
    // 게시물 상세
    @GetMapping("/{post_id}")
    public String detail(@PathVariable("post_id") String id) {
        if(id.equals("404")){
            throw new PostNotFound();
        }
        return "게시물 상세 : " +  id + "\n";
    }
    // 게시물 생성
    @PostMapping("/")
    @ResponseStatus(HttpStatus.CREATED)
    public String create(@RequestBody(required = false) String body){
        return "게시물 생성 : " + body + "\n";
    }
    // 게시물 수정
    @PatchMapping("/{post_id}")
    public String update(@PathVariable("post_id") String id) {
        return "게시물 수정 : " + id + "\n";
    }
    // 게시물 삭제
    @DeleteMapping("/{post_id}")
    public String delete(@PathVariable("post_id") String id) {
        return "게시물 삭제 : " + id + "\n";
    }

    @ExceptionHandler(PostNotFound.class)
    @ResponseStatus(HttpStatus.NOT_FOUND) 
    public String postNotFound(){
        return "게시물을 찾을 수 없습니다.";
    }
}
```

```java
public class PostNotFound extends RuntimeException {
}
```
### @RestController

- @RestController는 클래스가 REST API를 구현하는 컨트롤러 클래스라고 명시하는 어노테이션이다. @RestController는 @ResponseBody를 포함하고 있다.<br>

### @RequestMapping

- @RequestMapping은 공통된 URL을 표현할 때 사용하는 어노테이션이다.<br>위 예제를 보면 @RequestMapping("/posts") 라고 작성되었는데 이는 아래 다양한 collection pattern으로 표현된 URI의 맨 앞에 /posts를 공유한다. 그래서 각 어노테이션 마다 /posts를 일일이 입력할 수고를 덜어준다.<br><br>

### HTTP Method처럼 생긴 어노테이션들

스프링에는 HTTP Method의 기능을 구현하는 어노테이션을 제공한다.<br>

|어노테이션|HTTP Method|
|-------|-----------|
|@GetMapping|GET|
|@PostMapping|POST|
|@PatchMapping|PATCH|
|@PutMapping|PUT|
|@DeleteMapping|DELETE|

표에 있는 어노테이션들은 대응된 HTTP Method와 같은 기능을 한다고 생각하면 된다.

### @PathVariable

```java
@GetMapping("/{post_id}")
    public String detail(@PathVariable("post_id") String id) {...생략...}
```

- @GetMapping에 있는 {post_id}는 @PathVariable이 선언된 매개 변수에 바인딩 처리를 할 수 있다. 다시말해 URI의 리소스가 /posts/abc로 요청을 하면 매개변수인 id에 abc의 값이 들어가게 된다.<br><br>

### @ResponseStatus

- @ResponseStatus는 HTTP 응답코드를 반환하는 어노테이션이다.
- 예제에서는 @ResponseStatus(HttpStatus.CREATED)가 사용되었는데 HTTPie를 사용하여 응답코드를 받으면 201을 반환한다.<br><br>

### @ExceptionHandler

- @ExceptionHandler는 예외처리 핸들러로 직접 만든 예외처 응답을 클라이언트에게 제공하는 어노테이션이다.
```java
    @ExceptionHandler(PostNotFound.class)
    @ResponseStatus(HttpStatus.NOT_FOUND) 
    public String postNotFound(){
        return "게시물을 찾을 수 없습니다.";
    }
```

- 예제에서 PostNotFound.class로 예외를 던지면 @ExceptionHandler가 선언된 postNotFound() 메서드의 반환값인 "게시물을 찾을 수 없습니다." 를 응답 본문에 띄워 예외를 처리한다.
- @ResponseStatus(HttpStatus.NOT_FOUND)<br>
=> 응답코드는 NOT_FOUND로 지정했으므로 404를 반환할 것이다.

