# 직렬화

## 직렬화(Serialization)
- 데이터 저장 영역 내 표시되는 코드와 데이터 객체를 바이트 코드로 변환하여 객체의 상태를 쉽게 전송할 수 있는 형태로 저장하는 프로세스<br><br>
직렬화 프로세스<br>
![serialization](https://hazelcast.com/wp-content/uploads/2021/12/serialization-diagram-800x364-1.png)
<br><br>
- 객체를 DB에 저장하거나 네트워크로 전송하기 위해서는 객체를 복구할 수 있도록 데이터화 하는 것이 필요하다.
- 객체를 데이터화 하는 방법
  - 바이너리 &rarr; **Byte Stream**로 변환
  - 텍스트 &rarr; **XML, JSON, YAML**과 같은 데이터 포맷으로 파싱

 파싱(Parsing) : 문자열에서 네이티브 객체로 변환하는 것<br>

## 역직렬화(Deserialization)
- 일련의 바이트에서 데이터 구조 또는 객체로 변환하는 역방향 프로세스<br>
객체를 다시 생성하여 프로그래밍 언어의 기본 구조로 데이터를 더 쉽게 읽고 수정할 수 있도록 한다.


역질렬화 프로세스<br>
![Deserialization](https://hazelcast.com/wp-content/uploads/2021/12/serialization-deserialization-diagram-800x318-1.png)
<br>

- 직렬화와 역직렬화는 함께 동작하여 데이터 객체를 이식 가능한 형식으로 변환/ 재작성한다.

**[직렬화 참고사이트](https://hazelcast.com/glossary/serialization/)**

## 마샬링
- 객체의 메모리 표현을 저장 또는 전송에 적합한 데이터 형식으로 변환하는 프로세스로 프로그램의 다른 부분 간에 데이터 이동에 사용되거나 한 프로그램에서 다른 프로그램으로 데이터를 이동에 사용된다
- JAVA에서는 원격 호출을 위해 객체를 직렬화할 때 사용한다.
- 객체를 "마샬링" 한다는 것은 원복 객체의 복사본을 얻도록 객체의 상태와 코드베이스를 기록하는 것을 의미한다.<br> 

코드베이스 : 소스코드가 아닌 객체코드를 로드할 수 있는 URL 목록
- 다른 Java 가상 머신의 객체에서 메서드를 호출하려면 java.rmi.Remote 인터페이스를 구현한다.<br>

[마샬링 참고사이트](https://en.wikipedia.org/wiki/Marshalling_(computer_science))
<br><br>

## JSON(Javascript Object Notation)

사람이 읽을 수 있는 텍스트를 사용하여 속성-값 쌍과 배열(또는 기타 직렬화가 가능한 값)로 구성된 데이터 객체를 저장 및 전송하는 개방형 표준 파일 형식이자 데이터 교환 형식<br>
간단히 말하자면 사람이 이해하기 쉬운 데이터 포맷<br><br>

보안 문제로 인하여 JSON.parse(역직렬화), JSON.stringify(직렬화)로 안전하게 사용한다.<br><br>
JSON 기본 형태<br>
`{ key : value }`<br>

```
// 예시 
{ "squadName": "Super hero squad"}
```
Java에서는 Map이 이와 유사하지만, 스키마 관리 및 타입 안정성을 위해 DTO를 활용한다.
- 생성 : DTO &rarr; 변환기 &rarr; JSON 문자열
- 해석 : JSON 문자열 &rarr; 변환기 &rarr; DTO

[JSON 참고사이트](https://en.wikipedia.org/wiki/JSON)