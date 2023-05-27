# HTTP Server

### java.net.ServerSocket

ServerSocket은 클라이언트/서버 소켓 연결의 서버 측에 대한 시스템 독립적인 구현을 제공하는 java.net 클래스라고 공식문서에서 설명하고 있다.<br>
간단하게 서버에서 사용하는 Socket 이라고 생각하면 된다.<br><br>

![](https://www.ibm.com/docs/ko/ssw_ibm_i_73/rzab6/rxab6500.gif)&nbsp; [출처:IBM](https://www.ibm.com/docs/ko/i/7.3?topic=programming-how-sockets-work)<br>

위 그림은 서버와 클라이언트가 어떻게 연결을 맺는지에 대한 간단한 그림이다.<br><br>

### ServerSocket 를 사용하여 서버에 연결하기

1. 자바에서 서버는 클라이언트의 요청을 받기 위해 ServerSocket 클래스를 사용한다.
그리고 소켓 프로그래밍을 하기 위해서는 서버의 IP주소와 포트번호가 필요하다. 여기서는 본인 PC에 접속할 것이기 때문에 포트번호만 사용한다. ]
```java
int port = 8080;
ServerSocket ServerSocket = new ServerSocket(port);
```
2. 서버는 클라이언트의 접속을 기다린다. accept() 메서드를 사용하고, 클라이언트가 성공적으로 연결되면 서버 측에서 Socket 인스턴스를 반환한다.
```java
Socket socket = serverSocket.accept();
```

3. Request<br>
클라이언트는 요청을 보내는 것이지만 ***서버*** 입장에서는 요청을 받는 것이기 때문에 읽어야한다(read).
```java
// 1. Socket에서 getinputStream()을 가져와서 Reader에 저장
Reader reader = new InputStreamReader(socket.getInputStream());
// 2. CharBuffer로 1_000_000을 할당한다.
CharBuffer charBuffer = CharBuffer.allocate(1_000_000);
// 3. 할당된 값을 Reader로 읽어드린다.
reader.read(charBuffer);
// 4. charBuffer는 항상 flip()을 해준다.
charBuffer.flip();
```
4. response<br>
서버 입장에서 요청을 받았으면 응답을 해줘야한다. 서버는 응답 메세지를 만들어서 전송 해준다.
```java
// 1. Socket에서 getOutputStream()을 가져와서 Writer에 저장
Writer writer = new OutputStreamReader(socket.getOutputStream());
// 2. 응답 body를 만들어서 byte[] 배열로 저장
            String body = """
                        <!DOCKTYPE html>
                        <html>
                            <head lang="ko">
                                <title>예제</title>
                            </head>
                            <body>
                                <p>Hello World!</p>
                            </body>
                        </html>
                    """;

            byte[] bytes = body.getBytes();
// 3. 응답 메세지 생성
            String message = "" +
                    "HTTP/1.1 200 OK\n" +
                    "Content-Type: text/html; charset=UTF-8\n" +
                    "Content-Length: " + bytes.length + "\n" +
                    "\n" +
                    body;
// 4. Writer로 message를 쓴다.
writer.write(message);

// 5. 꼭 writer를 flush() 해준다. 
writer.flush();

// 6. 소켓을 다 썼으면 항상 close() 해준다.
socket.close();
ServerSocket.close();

```
---

### Blocking & Non-Blocking

I/O 에서는 기다리는 것을 Blocking 이라 한다. 위 예제에서 accept()는 Blocking 메서드이다. 클라이언트가 서버에 연결하기 전까지 차단한다<br>
항상 연결을 기다리기까지 다른 작업을 차단한다면 비효율적이다. 그래서 필요한 것이 Non-Blocking 이다.<br><br>
Non-Blocking 은 클라이언트가 요청을 하지만 요청한 작업이 끝날 때 까지 기다리지 않는다. 그 사이에 다른 작업을 할수가 있다.<br>

[Blocking vs Non-Blocking 참고자료](https://www.javatpoint.com/java-nio-vs-input-output)






