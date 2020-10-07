# Servlet

## Servlet의 개념
자바를 사용하여 웹페이지를 동적으로 생성하는 서버측 프로그램 혹은 그 사양을 말함   
웹 기반의 요청에 대한 동적인 처리가 가능한 하나의 클래스

### Servlet Program의 기본적인 동작과정
<img src ="https://gmlwjd9405.github.io/images/web/servlet-program.png"></img>
1. Web Server는 HTTP request를 Web Container(Servlet Container)에게 위임한다.
  1) web.xml 설정에서 어떤 URL과 매핑되어 있는지 확인
  2) 클라이언트(browser)의 요청 URL을 보고 적절한 Servlet을 실행
  *  https://gmlwjd9405.github.io/2018/10/29/web-application-structure.html 참고

2. Web Container는 service() 메서드를 호출하기 전에 Servlet 객체를 메모리에 올린다.
  1) Web Container는 적절한 Servlet 파일을 컴파일(.class 파일 생성)한다.
  2) .class 파일을 메모리에 올려 Servlet 객체를 만든다.
  3) 메모리에 로드될 때 Servlet 객체를 초기화하는 init() 메서드가 실행된다.

3. Web Container는 Request가 올 때마다 thread를 생성하여 처리한다.
  * 각 thread는 Servlet의 단일 객체에 대한 service() 메서드를 실행한다.

#### Servlet Program에서 Thread의 역할
* Thread란? 운영체제로부터 시스템 자원을 할당받는 작업의 단위
  + https://gmlwjd9405.github.io/2018/09/14/process-vs-thread.html 참고
* Servlet Program에서 thread가 수행할 메서드가 지정/할당되면
  + thread는 생성 후 즉시 해당 메서드만 열심히 수행한다.
  + 해당 메서드가 return하면 thread는 종료되고 제거된다.
  + 즉, 실제로 thread의 역할: Servlet의 doGet() 또는 doPost()를 호출하는 것이다.

* Web Container(Servlet Container)는 thread의 생성과 제거를 담당한다.
  + 하지만 thread의 생성과 제거의 반복은 큰 오버헤드를 만든다.
  + 이를 위해 Tomcat(WAS)은 “Thread Pool”(미리 thread를 만들어 놓음) 이라는 적절한 메커니즘을 사용하여 오버헤드를 줄인다.* 
* 즉, WAS는 Servlet의 life cycle을 담당한다.
  + 웹 브라우저 클라이언트의 요청이 들어왔을 때 Servlet 객체 생성은 WAS가 알아서 처리한다.
  + WAS 위에서 Servlet이 돌아다니고 개발자는 이 Servlet을 만들어야 한다.
