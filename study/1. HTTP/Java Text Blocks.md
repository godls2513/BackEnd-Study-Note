# JAVA Text Blocks

### Java Text Blocks
<br>

Java Text Blocks는 자바 15에서 탄생한 여러 줄의 문자열을 효율적으로 선언하는 방법이다. 

사용법은 간단하다. 
```java
String message = """
            GET / HTTP/1.1
            Host: example.com

            """;
```
텍스트 블록안에서는 줄 바꿈 이스케이프 없이도 개행과 따옴표를 자유롭게 사용할 수가 있다.

자바 15버전 이하에서는 다음과 같이 불편하게 사용하였다.
```java
String message = "" +
        "GET / HTTP/1.1" +
        "Host: example.com" +
        "\n";
```

기존에는 위와 같이 개행을 해주려면 개행문자(\n)가 들어가야하고 줄이 바뀔 때 마다 +를 써줘야한다. <br><br>
문자열의 개수가 작아서 그렇지 훨씬 많은 문자열이 들어간다면 확실히 텍스트 블록이 훨씬 간편해 보인다.

이렇게 간단하게 Java Text Blocks에 대해 알아보았다. 자세한 내용은 [Java_Text_Blocks](https://www.baeldung.com/java-text-blocks) 에서 배워보자!
