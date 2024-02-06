# HTTP Client

### TCP/IP 통신
<br>

[인터넷 프로토콜 스위트](https://ko.wikipedia.org/wiki/%EC%9D%B8%ED%84%B0%EB%84%B7_%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C_%EC%8A%A4%EC%9C%84%ED%8A%B8)<br>
: 인터넷에서 컴퓨터들이 서로 정보를 주고받는 데 쓰이는 통신규약(프로토콜) 일반적으로 TCP와 IP가 가장 많이 쓰이므로 TCP/IP 프로토콜이라고도 불린다.<br>

[전송계층](https://ko.wikipedia.org/wiki/%EC%A0%84%EC%86%A1_%EA%B3%84%EC%B8%B5)<br>
: 계층 구조의 네트워크 구성요소와 프로토콜 내에서 송신자와 수신자를 연결하는 **통신 서비스**를 제공하는 계층<br>
대표적인 프로토콜로는 **TCP**와 **UDP**가 있다.

---

### TCP와 UDP
<br>

[TCP(Transmission Control Protocol, TCP)](https://ko.wikipedia.org/wiki/%EC%A0%84%EC%86%A1_%EC%A0%9C%EC%96%B4_%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C)<br>
-  연결 지향 방식을 사용하는 전송 제어 프로토콜
-  근거리 통신망이나 인트라넷, 인터넷에 연결된 컴퓨터에서 실행되는 프로그램간 일련의 옥텟을 안정적으로, 순서대로, 에러없이 교환할 수 있게 한다. (연결 필요, 전달 및 순서 보장)<br>
ex) 전화

[UDP(User Datagram Protocol, UDP)](https://ko.wikipedia.org/wiki/%EC%82%AC%EC%9A%A9%EC%9E%90_%EB%8D%B0%EC%9D%B4%ED%84%B0%EA%B7%B8%EB%9E%A8_%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C)<br>
- TCP보다 단순한 전송에 사용되는 사용자 데이터그램 프로토콜
- 서비스의 신뢰성이 낮고, 데이터그램 도착 순서가 바뀌거나, 중복되거나, 심지어는 통보 없이 누락시키기도 한다.<br>
ex) 편지

UDP와 TCP 비교<br>
&nbsp; TCP : 설정된 **연결**을 통해 **양방향**으로 데이터를 전송<br>
&nbsp; UDP : 연결을 설정하지 않고 수신자가 데이터를 받을 준비를 확인하는 단계를 거치지 않고 **단방향**으로 정보를 전송

---

### Socket

[Socket](https://ko.wikipedia.org/wiki/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC_%EC%86%8C%EC%BC%93)<br>

&nbsp; 소켓이라 하면 **네트워크 소켓**을 말한다. (Socket API와 구분)<br>
&nbsp; 네트워크 소켓(network socket)은 컴퓨터 네트워크를 경유하는 프로세스 간 통신의 종착점(end point)이다.<br>
&nbsp; 대부분의 네트워크 소켓은 인터넷 소켓이고 대표적으로 **TCP**와 **UDP**가 있다.<br>
<br>

[소켓 API](https://www.ibm.com/docs/en/zos/2.3.0?topic=internets-socket-api)<br>
소켓 API는 애플리케이션 프로그램 간에 다음과 같은 주요 통신 기능을 수행할 수 있는 소켓 호출 모음이다.
- 네트워크에서 다른 사용자와의 연결 설정 및 설정
- 다른 사용자와 데이터 주고받기
- 연결 닫기

[Berkeley sockets](https://en.wikipedia.org/wiki/Berkeley_sockets)

---

### TCP 통신 순서

1. 서버는 접속 요청을 받기 위한 소켓을 연다. <br>
즉 클라이언트가 서비스를 요청하기를 기다리는 단계 → Listen
2. 클라이언트는 소켓을 만들고, 서버에 접속을 요청한다. → Connect
3. 서버는 접속 요청을 받아서 클라이언트와 통신할 소켓을 따로 만든다. → Accept
4. 소켓을 통해 서로 데이터를 주고 받는다. → Send & Receive ⇒ 반복!
5. 통신을 마치면 소켓을 닫는다. → Close ⇒ 상대방은 Receive로 인지할 수 있다.

![](https://www.ibm.com/docs/ko/ssw_ibm_i_73/rzab6/rxab6500.gif)&nbsp; [출처:IBM](https://www.ibm.com/docs/ko/i/7.3?topic=programming-how-sockets-work)<br>

---
### URI와 URL

[URI(Uniform Resource Identifier)](https://developer.mozilla.org/ko/docs/Glossary/URI)<br>
&nbsp; 하나의 리소스를 가리키는 문자열 또는 인터넷에 있는 자원을 나타내는 유일한 주소로 가장 흔한 URI는 URL로, 웹 상의 위치로 리소스르 식별한다.<br>
[URL(Uniform Resource Locator)](https://ko.wikipedia.org/wiki/URL)<br>
&nbsp; 인터넷에서 웹 페이지, 이미지, 비디오 등 리소스의 위치를 가리키는 문자열으로 HTTP 맥락에서 URL은 "웹 주소" 또는 "링크"라고 불린다.<br>
&nbsp; 쉽게 말해서, 웹 페이지를 찾기위한 주소를 말한다. &rarr;  http://example.com <br>

---

### Host
[Host](https://en.wikipedia.org/wiki/Host_(network))<br>
호스트란 컴퓨터 네트워크에 연결된 컴퓨터나 기타 장치로 호스트에는 하나 이상의 네트워크 주소가 할당됩니다.

[IP 주소](https://www.techtarget.com/whatis/definition/IP-address-Internet-Protocol-Address)<br>
인터넷에 연결되는 장치 또는 네트워크에 대한 고유한 식별자이다.
<br>
&nbsp; ex) 192.0.2.1 <br>
IP주소는 host를 식별하고 네트워크에서 호스트의 위치를 제공하며 host에 대한 경로를 설정할 수 있는 기능을 제공한다.<br>

[Domain name](https://en.wikipedia.org/wiki/Domain_name)
<br>
도메인 네임은 영숫자 IP 주소에 매핑되는 텍스트 문자열로, 클라이언트 소프트웨어에서 웹 사이트에 액세스하는 데 사용된다.<br>
예를 들어 IP주소가 192.0.2.1을 가진 도메인 네임이 google.com 이면
사용자는 편리하게 google.com을 주소창에 입력하여 google에 접속할 수가 있다. 이렇게 복잡한 IP주소를 도메인 네임으로 변환하는 시스템을 DNS라고 한다.

[DNS(Domain Name System)](https://en.wikipedia.org/wiki/Domain_Name_System)<br>
DNS(도메인 이름 시스템)는 인터넷 도메인 이름을 찾아 인터넷 프로토콜(IP) 주소로 변환하는 명명 시스템이다.

[PORT](https://en.wikipedia.org/wiki/Port_(computer_networking))<br>
특정 서비스에 대한 통신을 수신하거나 전송하는 네트워크 프로토콜과 관련된 소프트웨어 정의 번호

[Path(경로)](https://en.wikipedia.org/wiki/Port_(computer_networking))<br>
디렉토리 구조에서 위치를 고유하게 식별하는 문자열로 hostname:/directorypath/resource 와 같이 슬래시("/")로 구분하여 위치를 식별할 수 있으며 운영체제에 따라 슬래시("/"), 역슬래시("\") 등 다양한 구분 문자로 위치를 식별한다.