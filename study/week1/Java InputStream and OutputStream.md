# Java InputStream and OutputStream

### [Java InputStream](https://docs.oracle.com/javase/8/docs/api/java/io/InputStream.html)

```java
public abstract class InputStream
extends Object
implements Closeable
```

Java의 InputStream은 추상 클래스로 바이트 단위의 입력 스트림을 나타내는 모든 클래스의 슈퍼클래스이다. 그래서 서브클래스에서 데이터를 읽을 수 있도록 구현을 해야한다.<br>
대표적인 서브클래스는 다음과 같다.<br>
>AudioInputStream, ByteArrayInputStream, FileInputStream, FilterInputStream, InputStream, ObjectInputStream, PipedInputStream, SequenceInputStream, StringBufferInputStream

### InputStream의 여러가지 메서드

InputStream은 각 서브클래스마다 다양한 메서드들을 가진다. 아래는 그 중 일반적으로 사용되는 메서드들이다.

>read() : 입력 스트림에서 1바이트의 데이터를 읽습니다.<br>
read(byte[] array) - 스트림에서 바이트를 읽고 지정된 배열에 저장합니다.<br>
available() - 입력 스트림에서 사용 가능한 바이트 수를 반환합니다.<br>
mark() - 입력 스트림에서 데이터를 읽은 위치를 표시합니다.<br>
reset() - 마크가 설정된 스트림의 지점으로 컨트롤을 반환합니다.<br>
markSupported() - 스트림에서 mark() 및 reset() 메서드가 지원되는지 확인합니다.<br>
skips() - 입력 스트림에서 지정된 바이트 수를 건너뛰고 버립니다.<br>
close() - 입력 스트림을 닫습니다.

### FileInputStream을 이용한 InputStream 예제

다음은 서브클래스인 FileInputStream을 이용하여 test.txt의 내용을 읽어오는 예제이다. 

```java
import java.io.FileInputStream;
import java.io.IOException;

public class InputStreamDemo {
    public static void main(String[] args) throws IOException {
        String path = "app/src/main/resources/test.txt";
        byte[] bytes = new byte[100]; // 크기가 100인 바이트 배열을 선언

        FileInputStream fileInputStream = new FileInputStream(path);
        System.out.println("파일에서 사용 가능한 바이트 수 : " + fileInputStream.available());

        // 입력스트림에서 바이트 읽기
        fileInputStream.read(bytes);
        System.out.println("데이터 읽기 : ");

        // 바이트 배열을 문자열로 변환
        String data = new String(bytes);
        System.out.println(data);

        // 입력 스트림 닫기
        fileInputStream.close();
    }
}
```
결과
![](/study/week1/image/InputStreamResult.jpg)

