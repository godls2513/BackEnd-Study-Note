# 제어의 역전 컨테이너와 의존성 주입 패턴 (Inversion of Control Containers and the Dependency Injection pattern)
## 컴포넌트와 서비스
**컴포넌트**<br><br>
여기서 컴포넌트란 컴포넌트 작성자의 통제를 벗어난 애플리케이션에서 변경없이 사용하도록 의도된 소프트웨어의 집합을 의미합니다. '변경 없이'란 컴포넌트 작성자가 허용한 방식으로 확장하여 컴포넌트의 동작을 변경할 수는 있지만, 사용 애플리케이션이 컴포넌트의 소스 코드를 변경하지 않는다는 의미입니다.<br><br>
**서비스**<br><br>
서비스는 외부 애플리케이션에서 사용된다는 점에서 컴포넌트와 유사합니다. 차이점은 컴포넌트는 로컬에서 사용된다는 점이고 서비스는 동기식 또는 비동기식 원격 인터페이스 예를들면 웹서비스나 메시징 시스템, RPC 또는 소켓과 같은 것들을 통해 원격으로 사용됩니다.<br><br>
## 예시
예시는 특정 감독이 감독한 영화 목록을 제공하는 컴포넌트를 작성하고 있습니다. 여기서 함수는 단일 메서드로 구성하고 있습니다.<br>
```java
class MovieLister {
  public Movie[] moviesDirectedBy(String arg) {
    List allMovies = finder.findAll();
    for(Iterator it = allMovies.iterator();
        it.hasNext();) {
          Movie movie = (Movie) it.next();
          if(!movie.getDirector().equals(arg))
            it.remove();
        }
        return (Movie[]) allMovies.toArray(new Movie[allMovies.size()]);
  }
}
``` 
위 메서드의 구현은 finder 객체에서 알고있는 모든 영화를 반환하도록 요청합니다.
```java
 List allMovies = finder.findAll();
```
그런 다음 목록을 검색하여 특정 감독이 감독한 영화를 반환합니다.<br><br> moviesDirectedBy 메서드는 모든 영화가 저장되는 방식과 완전히 독립적이어야합니다. 그렇게 하기 위해서는 MovieLister 객체를 특정 finder 객체와 연결하는 것입니다. moviesDirectedBy 메서드가 하는 일은 finder를 참조하는 것뿐이고, finder는 finderAll 메서드에 응답하는 방법만 알면 됩니다. 이것은 finder에 대한 인터페이스를 정의하면 해결할 수가 있습니다.<br>
```java
  public interface MovieFinder {
    List findAll();
  }
```
실제로 영화를 만들려면 구체적인 클래스를 만들어야합니다. 이는 MovieLister 클래스의 생성자에 이를 위한 코드를 넣습니다.
```java
class MovieLister {
  private MovieFinder finder;

  public MovieLister() {
    finder = new ColonDelimitedMovieFinder("movies1.txt");
  }

  public Movie[] moviesDirectedBy(String arg) {
    List allMovies = finder.findAll();
    for(Iterator it = allMovies.iterator();
        it.hasNext();) {
          Movie movie = (Movie) it.next();
          if(!movie.getDirector().equals(arg))
            it.remove();
        }
        return (Movie[]) allMovies.toArray(new Movie[allMovies.size()]);
  }
}
```
생성자에 ColonDelimitedMovieFinder 클래스를 생성하면서 finder를 구현하였습니다. ColonDelimitedMovieFinder 클래스는 인자로 들어오는 텍스트 파일에 콜론으로 구분된 영화를 반환합니다. (이 클래스의 구현이 중요한게 아니지만...)<br>
그런데 여기서 영화목록을 저장하는 형식이 텍스트 파일이 아닌 SQL 데이터베이스, XML, 웹 서비스 등 완전히 다른 형식이라면 문제가 됩니다. 이 경우 해당 데이터를 가져올 다른 클래스가 필요합니다.

![](https://martinfowler.com/articles/injection/naive.gif)<br>
그림 1: 리스너 클래스에서 간단한 생성을 사용하는 의존성<br><br>
Movielister 클래스는 MovieFider 인터페이스와 구현에 모두 의존하고 있습니다. 이를 실제 시스템으로 확장하면 이러한 서비스와 컴포넌트가 수십 개가 될 수 있습니다. 여기서는 인터페이스에 의존하는 컴포넌트는 하나이지만 여러개라면 문제가 생깁니다. 이는 인터페이스가 컴포넌트와 대화함으로써 컴포넌트의 사용을 추상화하여 해결할 수가 있습니다. 위에서 언급했던 것처럼 다른 방식으로 배포하려면 어떻게 해야할까요?<br><br>**플러그인**을 사용하여 서비스와의 상호작용을 처리해야 배포마다 다른 구현을 사용할 수 있습니다. 이러한 **플러그인**을 애플리케이션과 조립하기 위해서는 **제어의 역전**이라는 용어가 등장합니다.<br>
## 제어의 역전(Inversion of Control, IoC)
애플리케이션의 제어권이 사용자가 아닌 프레임워크로 이동한 것을 **제어의 역전**(Inversion of Control)이라고 합니다. 위 예제에서 MoviewLister 객체가 직접 인스턴스화하여 finder 구현을 조회했습니다. 이것은 finder가 플러그인이 되는 것이 아닙니다. 따라서 플러그인 사용자가 별도의 어셈블러 모듈이 MovieLister에 구현을 주입할 수 있도록 하는 규칙을 따르도록 하는 것입니다. 이와 맞추어 제어의 역전이라는 용어는 너무 일반적이어서 다른 이름이 필요했습니다. 그래서 IoC의 옹호자들은 **의존성 주입**(Dependency Injection) 이라는 용어를 만듭니다.
## 의존성 주입(Dependency Injection)의 형태
의존성 주입의 기본 아이디어는 MovieLister 클래스의 필드를 finder 인터페이스에 구현으로 채우는 별도의 객체, 즉 어셈블러(assembler)를 사용하는 것입니다.<br><br>
![](https://martinfowler.com/articles/injection/injector.gif)
의존성 주입에는 세가지 스타일이 있습니다. **생성자 주입**(Constructor Injection), **설정자 주입**(Setter Injection), **인터페이스 주입**(Interface Injection) 입니다.
### 생성자 주입 (Constructor Injection)
여기서는 PicoContatiner라는 경량 컨테이너를 사용하여 생성자 주입이 어떻게 사용되는지 알아보겠습니다.( PicoContatiner의 구체적인 내용은 알 필요가 없습니다. )<br><br>
생성자를 사용하여 MovieLister 클래스에 finder의 구현을 주입하려면 MovieLister 클래스는 주입해야하는 모든 것을 포함하는 생성자를 선언해야합니다.
```java
  class Movielister {
    public MovieLister(MovieFinder finder) {
      thils.finder = finder;
    }
  }
```
finder 자체도 PicoContainer에 의해 관리되므로 컨테이너에 의해 텍스트 파일명이 주입됩니다.
```java
  class ColonMovieFinder {
    public ColonMovieFinder(String filename) {
      this.filename = filename;
    }
  }
```
그런 다음 각 인터페이스에 연결할 구현 클래스와 finder에 주입할 문자열을 PicoContainer에게 알려야 합니다.
```java
  private MutablePicoContainer configuerContainer() {
    MutablePicoContainer pico = new DefaultPicoContainer();
    Parameter[] finderParams = {new ConstantParameter("movie1.txt")};
    pico.registerComponentImplementation(MovieFinder.class, ColonMovieFinder.class, finderParams);
    pico.registerComponentImplementation(MovieLister.class);
    return pico;
  }
```
이 설정코드는 일반적으로 다른 클래스에서 설정됩니다. 예를 들어, 내 MovieLister를 사용하는 각 친구는 각자의 설정 클래스에서 적절한 코드를 작성할 수 있습니다. 이 프로젝트의 철학은 구성 파일 형식을 기본 매커니즘에서 분리하는 것입니다.<br><br>
컨테이너를 사용하려면 다음과 같이 할 수 있습니다.
```java
  public void testWithPico() {
    MutablePicoContainer pico = configureContainer();
    Movielister lister = (MovieLister)pico.getComponentInstance(MovieLister.class);
    Movie[] movies = lister.moviesDirectedBy("Sergio Leone");

    assertEqauls("Once Upon a Time in the West", movies[0].getTitle());
  }
```



