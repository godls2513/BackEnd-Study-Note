# HTTP Web Server 만들기

- 개요<br>
Java HttpServer와 Spring Web MVC를 사용하지 않고 순수하게 HTTP 웹서버를 만들어 보자.<br>

그럼 무엇을 통해서 만들 수 있을까?<br>
-> java.net.**ServerSocket** 을 이용하여 로우레벨로 만들 수 있다.<br>

1. gradle project 생성<br>

필자는 CUI 환경에서 gradle 프로젝트를 생성할 것이다. 우선 적당한 경로에 디렉토리를 생성하고, 해당 디렉토리에서 gradle init 명령어를 실행한다. <br>
``` Bash
mkdir small-http-server
cd small-http-server
gradle init
```
gradle init 이후 세팅은 아래 그림과 같다.<br>
![gradleInit](/study/week1/image/gradleInit.jpg)

small-http-server 디렉토리에 위치하고 idea . 명령어로 idea를 실행한다.

```Bash
idea .
```

프로젝트 빌드가 완료되면 App.java 파일이 생성된 것을 확인 할 수 있다. 이 클래스에서 오늘의 Http Server를 만들어 볼 것이다.<br>

Http Server를 만들기 이전에 서버는 어떤 식으로 돌아가는지 알 필요가 있다.<br>
1. 서버는 클라이언트의 요청을 기다린다. **listen**
2. 클라이언트의 요청이 들어오면 요청을 받는다. **accept**
3. 요청을 처리한다. **request**
4. 처리한 요청을 클라이언트에게 응답한다. **response**
5. 서버를 닫는다. **close**

간략하게 이런 식으로 작동한다고 생각하면 된다. 자세한 내용은 [여기](https://www.ibm.com/docs/ko/i/7.3?topic=programming-how-sockets-work)에서 확인<br>

2. App 클래스에서 프로젝트 시작하기

```java
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.Reader;
import java.net.ServerSocket;
import java.net.Socket;
import java.nio.CharBuffer;
import java.util.HashMap;
import java.util.Map;

public class App {
    private static Long newId = 0L;

    public static void main(String[] args) {
        App app = new App();
        app.run();
    }

    private void run() {
        // 여기서 HttpServer를 만들기 위한 로직을 구현
    }
}
```

우선 Long 타입의 Static 변수 newId를 0L으로 초기화 한다.
그 다음 App 객체를 생성하고 run() 이라는 메소드를 호출한다.
<br>여기서 run() 메소드가 HttpServer를 구현하기 위한 메소드이다.<br><br>

3.  run() 메소드 작성하기

```java
private void run() throws IOException {
    int port = 8080;
    int backlog = 0;

    Map<Long, String> tasks = new HashMap<>();
    
    // 1. Listen
    ServerSocket listener = new ServerSocket(port, backlog);
    System.out.println("listen!!");
    // 2. Accept
    while (true) {
        Socket socket = listener.accept();
        System.out.println("accept!!");
        // 3. Request
        String request = getRequest(socket);
        System.out.println("request!!");
        // 4. Response
        String message = process(tasks, request);
        writeMessage(socket, message);
        System.out.println("response!!");
    }
}
```
run() 메소드는 HttpServer를 구현하기 위한 메소드이다. int 타입의 port와 backlog를 각각 8080과 0으로 초기화 한다. 그리고 tasks라는 이름으로 HasMap 객체도 만들어 둔다.(tasks는 처리할 작업이다.)<br>
앞에서 서버가 어떻게 요청을 받고 응답을 하는지 알아보았다. 그 과정을 다시 간단하게 살펴보자.<br>
1. 서버가 클라이언트의 요청을 기다리는 **listen**
2. 요청을 받는 단계인 **accept**
3. 요청의 내용을 가져오는 **request**
4. 요청된 내용을 처리하여 응답하는 **response**

가장 먼저 listen 단계이다. java.net 클래스에 있는 ServerSocket 클래스에 매개변수로 port와 backlog를 사용하여 객체를 생성한다.
```java
ServerSocket listener = new ServerSocket(port, backlog);
```
이제 클라이언트의 요청을 받을 준비가 되었다. 그 다음 요청을 받기 위해 accept()를 사용하여 요청을 받을 수 있는 Socket을 만들어 준다.
```java
Socket socket = listener.accept();
```
이제 요청을 받아서 어떤 요청이 들어왔는지 읽어야한다. getRequest() 메서드를 통해 Socket에서 받아온 요청 내용을 읽어온다.
```java
String request = getRequest(socket);
```
아래는 Socket에서 요청을 읽어오는 getRequest() 메서드의 구현 소스이다<br><br>
### 요청 내용을 처리하기 위한 **getRequest(Socket socket)**
```java
private String getRequest(Socket socket) throws IOException {
    Reader reader = new InputStreamReader(socket.getInputStream());
    CharBuffer charBuffer = CharBuffer.allocate(1_000_000);
    reader.read(charBuffer);
    charBuffer.flip();
    return charBuffer.toString();

}
```
서버 입장에서는 요청은 받는 것이다. 따라서 받은 요청을 읽어야한다. Socket타입의 매개변수를 이용해서 inputStream으로 읽어오고 Reader에 저장한다. 
```java
Reader reader = new InputStreamReader(socket.getInputStream());
```
그 다음 CharBuffer를 1_000_000을 할당한다.
```java
CharBuffer charBuffer = CharBuffer.allocate(1_000_000);
```
Reader에서 CharBuffer를 읽는다. CharBuffer에 flip()을 해주고 CharBuffer를 문자열로 반환한다.
```java
reader.read(charBuffer);
charBuffer.flip();
return charBuffer.toString();
```
다시 run()메소드로 돌아가서 처리된 데이터를 클라이언트에게 응답해준다.
```java
String message = process(tasks, request);
writeMessage(socket, message);
```
클라이언트를 위한 응답 메세지를 만들기 위해 매개변수로 task, request를 전달하는 process 메서드를 생성해서 응답 메세지를 생성한다.
### 응답 메세지를 만들기 위한 process

```java
private static String process(Map<Long, String> tasks, String request) {
    String method = getRequestMethod(request);
    if (method.startsWith("GET /tasks")) {
        return processGetTask(tasks);
    }
    if (method.startsWith("POST /tasks")) {
        return processPostTask(tasks, request);
    }
    if (method.startsWith("PATCH /tasks")) {
        return processPatchTask(tasks, request, method);
    }
    if (method.startsWith("DELETE /tasks")) {
        return processDeleteTask(tasks, method);
    }

    return generateMessage("", "400 Bad Request");
}
```

요청 메서드를 가져오기 위해 getRequestMethod()라는 메서드를 새로 만들고 매개변수로 요청에서 가져온 request를 사용하여 가져온 요청 메서드를 문자열 method 변수에 저장한다.<br><br>
**요청 메서드를 가져오는 getRequestMethod**<br>
```java
private static String getRequestMethod(String request) {
    return request.substring(0, request.indexOf("HTTP"));
}
```
request를 맨 처음부터 문자열 "HTTP" 이전까지 읽어온 문자열을 반환한다.
예를 들어 요청 메서드가 "GET /index.html HTTP/1.1" 이면 "HTTP" 이전 부분인 "GET /index.html을 반환한다.<br>

그 다음 method가 각 조건으로 시작하면 각각 반환하는 메서드를 만든다. 메서드 조건 별 내용은 다음과 같다.<br>
"GET /tasks" 으로 시작할 때 processGetTask(tasks)<br>
```java
private static String processGetTask(Map<Long, String> tasks) {
    String content = new Gson().toJson(tasks);
    return generateMessage(content, "200 OK");
}
```
Map 타입의 매개변수 tasks를 Json 형태로 변환하여 문자열 content 변수에 저장한다. 그런 다음 generateMessage(content, "200 OK") 를 통해 

