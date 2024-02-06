# Java Lambda Expressions

람다표현식은 매개변수를 받아 값을 반환하는 짧은 표현식으로 메서드와 유사하지만 이름이 없고 메서드 본문에서 바로 구현가능한 표현식이다.<br>
람다표현식은 기본적으로 함수적 인터페이스를 구현한다.<br>

**함수적 인터페이스** : 추상 메서드가 하나만 있는 인터페이스

함수적 인터페이스의 예제
```java 
import java.lang.FunctionalInterface;
@FunctionalInterface
public interface MyInterface{
    // the single abstract method
    double getValue();
}
```

위 인터페이스는 추상메서드 getValue() 하나만 있기 때문에 함수적 인터페이스이다. @FunctionalInterface는 함수적 인터페이스임을 명시하는 어노테이션이다. 따라서 2개 이상의 추상메서드를 가질 수 없지만, 꼭 @FunctionalInterface가 필수적으로 있을 필요는 없다.<br>

### 람다표현식 소개

람다 표현식은 기본적으로 이름이 없는(익명) 메서드이다. 람다표현식은 자체적으로 실행되지가 않고, 함수적 인터페이스를 구현하는데 사용된다.
<br><br>

### 람다표현식의 문법

1. 파라미터가 1개인 경우
```java
parameter -> expression
```
2. 파라미터가 2개 이상인 경우, 괄호로 묶어준다
```java
(parameter1, parameter2) -> expression
```
expression은 즉시 값을 반환해야해서 변수의 할당 또는 if, for문을 사용할 수가 없다.

3. 보다 복잡한 연산을 사용하는 경우 중괄호와 함께 코드 블럭을 사용하면된다.
```java
(parameter1, parameter2) -> { code block }
```
### 람다표현식의 사용 예제

1. 파라미터가 1개인 람다식 예제
```java
import java.util.ArrayList;

public class LambdaEx {
    public static void main(String[] args) {
        ArrayList<Integer> numbers = new ArrayList<Integer>();
        numbers.add(0);
        numbers.add(1);
        numbers.add(2);
        numbers.add(3);

        // 람다표현식을 이용하여 numbers에 있는 모든 element들을 가져와 출력할 수 있다.
        // 컨텍스트에서 해당 변수의 유형을 유추할 수 있는 경우 괄호를 반드시 사용해야 하는 것은 아니다.
        numbers.forEach(n -> System.out.println(n));
    }
}

```

2. 파라미터가 2개 이상인 람다식 예제
```java
@FunctionalInterface
interface Addable {
    int add(int a, int b);
}

public class LambdaEx2 {
    public static void main(String[] args) {

        // 파라미터가 2개인 람다식
        Addable ad1=(a,b)->(a+b);
        System.out.println(ad1.add(10,20));

        // 데이터 타입이 2개 파라미터를 사용하는 람다식
        Addable ad2=(int a, int b) -> a+b;
        System.out.println(ad2.add(100,200));
    }
}
```

[람다표현식 참고자료](https://www.programiz.com/java-programming/lambda-expression)

