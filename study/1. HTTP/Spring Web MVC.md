# Spring Web MVC

## Spring Boot
Spring Boot는 프로덕션에 바로 사용할 수 있는 Spring 기반 애플리케이션을 빠르게(그리고 자유롭게) 만들 수 있는 방법을 제공합니다. Spring 프레임워크를 기반으로 하며, 구성보다 규칙을 선호하며, 가능한 한 빨리 시작하고 실행할 수 있도록 설계되었습니다. (공식문서 소개)

## Spring initializer
[Spring initializer](https://start.spring.io/)는 Spring Boot 기반으로 프로젝트를 쉽고 빠르게 생성할 수 있도록 해준다.

## Web Server 와 Application Server

### Web Server
: 클라이언트의 요청을 받아서 정적 콘텐츠(HTML, 파일, 이미지, 동영상) 등을 클라이언트에게 응답하는 서버 또는 컴퓨터<br>
Web Server는 HTTP 요청과 응답만을 처리한다.<br><br>
![Web Server](/study/week1/image/WebServer.jpg)

### Application Server
: Application Server는 동적인 콘텐츠(비즈니스 로직, 애플리케이션에서 제공하는 특수 기능을 제공하기 위해 데이터를 변환)를 처리하는 서버 또는 컴퓨터<br>

Application Server는 Web Server를 내포하고 있다. 이로 인해 현재는 웹서버와 어플리케이션 서버 간의 경계가 모호하다는 지적도 있다.<br><br>
![Application Server](/study/week1/image/ApplicationServer.png)

**Web Server와 Application Server의 차이점**

| Web Server | Application Server|
|------------|-------------------|
| 정적 콘텐츠 전달   | 동적 콘텐츠 전달  |
| HTTP 프로토콜만 사용   | HTTP를 포함한 여러 프로토콜 사용  |
| 웹기반 애플리케이션만 제공   | 웹 및 엔터프라이즈 기반 애플리케이션 서비스 제공|
| 멀티스레드 지원 안함 | 멀티스레드 지원|
| 리소스를 적게 사용 | 웹서버에 비해 리소스를 많이 사용


**Web Server & Application Server 참고자료**<br>
https://www.infoworld.com/article/2077354/app-server-web-server-what-s-the-difference.html<br>
https://www.educative.io/answers/web-server-vs-application-server<br>
https://www.ibm.com/topics/web-server-application-server

### Apache Tomcat
자바 서블릿을 실행하고, 자바 서버 페이지 코드가 포함된 웹 페이지를 렌더링 및 전달하며, 자바 엔터프라이즈 에디션(Java EE) 애플리케이션을 제공하는 오픈 소스 애플리케이션 서버<br>
[Apache Tomcat 공식 홈페이지](https://tomcat.apache.org/)<br>

## Model View Controller

애플리케이션을 Model, View, Controller 3가지 논리적인 구성요소로 나누는 이키텍처이다.<br><br>
**각 구성요소의 특징**<br>

Model : 데이터 로직을 담당<br>
View : GUI를 담당, 모델에서 가져온 데이터를 표현하지만 모델에서 직접 데이터를 받아올 수 없고 컨트롤러에게 요청한다.<br>
Controller : 사용자의 요청에 의하여 Model에게서 데이터를 가져오고, View가 데이터를 요청하면 Model으로부터 가져온 데이터를 제공한다. 중재자 역할이라고 보면된다.<br><br>

![MVC](https://media.geeksforgeeks.org/wp-content/uploads/20220224160807/Model1.png)<br>

**Model View Controller의 특징**<br>
관심사의 분리 : Model, View, Controller가 각자의 역할이 분리되어 있어 대규모 어플리케이션에 분업과 유지보수를 하기에 아주 용이하다. 하지만 그만큼 복잡하다.<br>

**참고자료**<br>
https://developer.mozilla.org/en-US/docs/Glossary/MVC#model_view_controller_example<br>

https://www.geeksforgeeks.org/mvc-framework-introduction/<br>

https://www.tutorialspoint.com/mvc_framework/mvc_framework_introduction.htm<br>

https://towardsdatascience.com/everything-you-need-to-know-about-mvc-architecture-3c827930b4c1<br>





