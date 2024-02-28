# Jackson ObjectMapper

## Jackson ObjectMapper 란? 
Jackson ObjectMapper는 자바 객체를 JSON으로 직렬화 한다거나 JSON을 자바 객체로 역직렬화할 수 있도록 도와주는 Jackson 라이브러리의 클래스이다.<br><br>
Jackson ObjectMapper를 사용하기 위해서는 예전에는 pom.xml과 같은 곳에 dependency로 따로 설치를 해줘야했지만 스프링에서는 기본적으로 내장이 되어 있어서 여기서는 설치 방법은 생략한다.

## JSON 문자열에서 객체 읽어오기
간단한 테스트를 위해 클래스를 main 메서드를 실행할 수 있는 클래스를 만든다.<br>
``` java
public class _1ReadFromJsonString {
    public static void main(String[] args) throws IOException {
        ObjectMapper objectMapper = new ObjectMapper();

        String carJackson = "{\"brand\":\"KIA\",\"name\":\"K3\"}";

        Car car = objectMapper.readValue(carJackson, Car.class);

        System.out.println("car brand : " + car.getBrand());
        System.out.println("car name : " + car.getName());
    }
}
```
위 코드를 해석하면 아래와 같다.<br>
-  com.fasterxml.jackson.databind.ObjectMapper 객체를 생성한다.
- JSON 문자열을 담은 String 타입의 carJackson 변수를 선언한다.
- **readValue()** : JSON 데이터를 자바객체로 역직렬화하는 메서드이다.<br>
해석하면 JSON 문자열을 담은 carJackson 변수의 내용을 Car클래스의 타입으로 변환하는 것이다.

다음은 Car클래스의 내용이다.<br>
```java
public class Car {
    private String brand = null;
    private String name = null;
    private int doors = 0;

    public Car() {
    }

    public Car(String brand, String name, int doors) {
        this.brand = brand;
        this.name = name;
        this.doors = doors;
    }

    public String getBrand() {
        return brand;
    }

    public void setBrand(String brand) {
        this.brand = brand;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getDoors() {
        return doors;
    }

    public void setDoors(int doors) {
        this.doors = doors;
    }
}
```

## 자바 객체를 JSON으로 쓰기
이번에는 자바 객체로 된 데이터를 JSON으로 쓰는 예제를 만들어 보고자 한다.<br>
```java
public class _1WriteFromObject {
    public static void main(String[] args) throws JsonProcessingException {

        ObjectMapper objectMapper = new ObjectMapper();

        Car car = new Car();
        car.setBrand("BMW");
        car.setName("520i");
        car.setDoors(5);

        String json = objectMapper.writeValueAsString(car);
        System.out.println(json);
    }
```
위 코드를 해석하면 다음과 같다.
- JSON 데이터를 읽을 때와 마찬가지로 ObjectMapper 객체를 생성한다.
- Car 객체르 생성하고 setter를 이용하여 데이터를 셋팅한다.
- **writeValueAsString()** : 자바객체를 JSON 데이터로 쓰는 메서드이다.<br>car에 셋팅된 brand, name, doors 등을 JSON 데이터로 출력해준다.<br><br>
## 마치며
Jackson ObjectMapper는 위에서 설명한 것 말고도 여러가지 방법으로 자바객체를 JSON으로 직렬화하거나 JSON 데이터를 자바객체로 역직렬화 할 수 있다. Reader를 사용한다거나 FILE에 저장되어 있는 JSON 데이터를 읽어온다. <br><br>더 자세히 알아보고 싶으면 아래 참고에서 확인하자.

참고<br>
[Jackson Objectmapper by Jankov](https://jenkov.com/tutorials/java-json/jackson-objectmapper.html)<br>
[Jackson ObjectMapper에 대해 더 정리한 내 GitHub](https://github.com/godls2513/learnJava/tree/master/src/main/java/com/chafy/learn/backend/jackson)
