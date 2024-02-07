# JAVA HTTP Server

## [com.sun.net.httpserver](https://docs.oracle.com/en/java/javase/17/docs/api/jdk.httpserver/module-summary.html)

임베디드 HTTP 서버를 구축하는 데 사용할 수 있는 간단한 하이레벨 Http 서버 API<br>

### [Java IO vs Java NIO](https://www.geeksforgeeks.org/difference-between-java-io-and-java-nio/)

Java IO는 읽기 및 쓰기 작업을 수행하는 데 사용된다. 그 중 고속 I/O 연산을 하기 위해 JDK 4부터 Java NIO가 도입되었다.<br>

Java IO와 Java NIO의 차이점<br>
- Java IO는 스트림 지향 패키지로 스트림에서 한번에 하나 이상의 바이트를 읽을 수가 있고, 단방향 데이터 전송이다.<br>
- Java IO는 Blocking IO 이다. 즉, 데이터를 읽으면 읽는 동안에는 다른 작업이 차단되는 동기식 IO 또는 Blocking IO 이다.<br>

![](https://media.geeksforgeeks.org/wp-content/uploads/20200519194033/Javaio-2.png)

- Java NIO는 버퍼 지향 패키지로 데이터가 버퍼로 읽혀지고 이 버퍼에서 채널을 추가하여 처리가 이루어진다. 데이터가 버퍼로 읽혀지면 읽기 작업 중에 다른 작업을 할 수 있는 있어 양방향 통신이 가능하다.<br>
- Java NIO는 Non-Blocking IO로 read() 또는 write() 작업을 호출하여 작업하는 경우 해당 작업이 완료될 때까지 완전히 차단 되지 않고 다른 작업을 수행할 수 있는 비동기식 IO 또는 Non-Blocking IO이다.

![](https://media.geeksforgeeks.org/wp-content/uploads/20200519194302/javanio-2.png)

Channel : 채널은 엔티티와 버퍼 간의 효율적인 데이터 전송을 위한 매개체<br>

---
## Java HTTP Server 만들기

1. 서버 객체 만들기
```java
// InetSocketAddress로 포트번호를 가져오고, 백로그는 0으로 지정한다.
InetSocket Address = new InetSocketAddress(8080);
int backlog = 0;

// HttpServer 객체 생성
HttpServer httpServer = HttpServer.create(Address, backlog);
```
2. URL(정확히는 path)에 핸들러를 지정<br>
(여기에 IO를 위한 소스코드가 들어갈 것이다.)
```java
// createContext()에 "/" 라는 path에 이용할 exchange를 람다식으로 구현한다.
httpServer.createContext("/", (exchange) -> {
  // 1. Request
  // HTTP Method 가져오기
    String method = exchange.getRequestMethod(); 

    // HTTP path 가져오기
    String uri = exchange.getRequestURI(); 
    String path = uri.getPath();

    // Header 가져오기
    Headers headers = exchange.getRequestHeaders();
    for(String key : headers.keySet()){
      	System.out.println(key + ": " + headers.get(key));
    }

    // Body 가져오기
    // Body는 InputStream으로 가져온다.
    InputStream inputStream = exchange.getRequestBody();
    String body = new String(inputStream.readAllBytes()); // inputStream에서 body 데이터를 Byte로 가져와서 문자열로 반환하도록 한다.


  // 2. Response
  // 데이터를 Byte배열로 가져온다.
  String content = "Hello world!!";
  Bytes[] bytes = content.getBytes();

  // HTTP Status Code와 Content-Length 지정한다.
  exchange.sendResponseHeaders(200, byte.length);

  // 데이터를 전송한다.
  OutputStream outputStream = exchange.getReponseBody();
  outputStream.write(bytes);
  write.flush();

});

httpServer.start();
```

**결과**

![result](/study/1.%20HTTP/image/JavaHttpServer.jpg)


