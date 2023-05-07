# Java InputStream and OutputStream

### [Java InputStream](https://docs.oracle.com/javase/8/docs/api/java/io/InputStream.html)

```java
public abstract class InputStream
extends Object
implements Closeable
```

Java의 InputStream은 추상 클래스로 바이트 단위의 입력 스트림을 나타내는 모든 클래스의 슈퍼클래스이다.<br>

### InputStream의 여러가지 메서드

>read() : 입력 스트림에서 1바이트의 데이터를 읽습니다.<br>
read(byte[] array) - 스트림에서 바이트를 읽고 지정된 배열에 저장합니다.<br>
available() - 입력 스트림에서 사용 가능한 바이트 수를 반환합니다.<br>
mark() - 입력 스트림에서 데이터를 읽은 위치를 표시합니다.<br>
reset() - 마크가 설정된 스트림의 지점으로 컨트롤을 반환합니다.<br>
markSupported() - 스트림에서 mark() 및 reset() 메서드가 지원되는지 확인합니다.<br>
skips() - 입력 스트림에서 지정된 바이트 수를 건너뛰고 버립니다.<br>
close() - 입력 스트림을 닫습니다.


InputStream의 서브클래스를 정의해야 하는 애플리케이션은 항상 다음 바이트의 입력을 반환하는 메서드를 제공해야 한다. 그 중 오늘은 OutputStream

