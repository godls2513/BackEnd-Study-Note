# try-with-resources

### [try-with-resources](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html)

자바에서 리소스는 프로그램이 완료된 후 닫아야한다. 일반적으로는 close() 메서드를 사용한다.<br>
try-with-resources는 close() 메서드를 사용하지 않고 try-with-resources문을 빠져나가기만 하면 자동으로 리소스가 닫히도록 해주어 편리하게 사용할 수 가 있다.<br>
java.io.Colseable이나 java.lang.AutoCloseable을 구현하는 모든 객체에 이 try-with-resource문을 사용할 수가 있다.<br><br>

---

### try-with-resources 예제 코드

```java
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.io.Reader;
import java.net.Socket;
import java.nio.CharBuffer;

public class App {
    public App() throws IOException {
    }

    public static void main(String[] args) throws IOException {
        App app = new App();
        app.run();
    }

    private void run() throws IOException {
        System.out.println("Hello world!!");
        System.out.println();

        String host = "example.com";
        int port = 80;

        // 1. Connect
        try (Socket socket = new Socket(host, port)) {

            String message = "" +
                    "GET / HTTP/1.1" +
                    "Host: example.com" +
                    "\n";

            // 2. Request
            OutputStreamWriter writer = new OutputStreamWriter(socket.getOutputStream());
            writer.write(message);
            writer.flush();
            System.out.println("Request!!");

            // 3. Response
            Reader reader = new InputStreamReader(socket.getInputStream());
            CharBuffer charBuffer = CharBuffer.allocate(1_000_000); // CharBuffer로 1_000_000을 할당해줌
            reader.read(charBuffer); // Reader로 CharBuffer에 할당된 1_000_000을 읽음
            charBuffer.flip();
            System.out.println(charBuffer.toString());

            //socket.close(); // 소켓을 다 쓰면 더이상 할 게 없으므로 반드시 닫아준다. try-with-resources문 사용으로 필요가 없다.
            System.out.println("socket close!!");
        }
    }
}

```

위 코드는 소켓을 이용하여 HTTP Client 통신하는 예제이다.
```java
   try (Socket socket = new Socket(host, port)) {
      // request
      // response
   }
```
위 예시처럼 사용하고자 하는 객체를 try문으로 감싸서 사용하면 close() 메서드를 사용하지 않고 자동으로 리소스를 닫아준다.<br>

Socket이 try-with-resources를 사용할 수 있는 이유는 java.io.Closeable을 구현하는 객체이기 때문이다. <br>

자바 공식문서를 보면 Socket이 java.io.Closeable을 구현한다.
[Socket 자바 공식문서](https://docs.oracle.com/javase/8/docs/api/java/net/Socket.html)