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
