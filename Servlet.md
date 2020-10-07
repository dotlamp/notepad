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
  + 이를 위해 Tomcat(WAS)은 “Thread Pool”(미리 thread를 만들어 놓음) 이라는 적절한 메커니즘을 사용하여 오버헤드를 줄인다. 
* 즉, WAS는 Servlet의 life cycle을 담당한다.
  + 웹 브라우저 클라이언트의 요청이 들어왔을 때 Servlet 객체 생성은 WAS가 알아서 처리한다.
  + WAS 위에서 Servlet이 돌아다니고 개발자는 이 Servlet을 만들어야 한다.
  
### Servlet Life Cycle
<img src="https://gmlwjd9405.github.io/images/web/servlet-life-cycle.png"></img>
* 클라이언트의 요청이 들어오면 WAS는 해당 요청에 맞는 Servlet이 메모리에 있는지 확인한다.
  + 만약 메모리에 없다면 해당 Servlet Class를 메모리에 올린 후(Servlet 객체 생성) init 메서드 실행
    * 이후 service 메서드를 실행
  + 메모리에 있다면 바로 service 메서드 실행
```JAVA
     if (메모리에 없음) {
     // 해당 서블릿 클래스를 메모리에 올림
     // init() 메소드를 실행
     }
     // service()메소드를 실행
```
* init()
  + 한 번만 수행된다.
  + 클라이언트(browser)의 요청에 따라 적절한 Servlet이 생성되고 이 Servlet이 메모리에 로드될 때 init() 메서드가 호출된다.
  + 역할: Servlet 객체를 초기화

* service(request, response)
  + 응답에 대한 모든 내용은 service() 메서드에 구현해야 한다.
  + Servlet이 수신한 모든 request에 대해 service() 메서드가 호출된다.
    + HttpServlet을 상속받은 Servlet 클래스(이하 하위 클래스)에서 service() 메서드를 오버라이드하지 않았다면, 그 부모인 HttpServlet의 service()가 호출된다.
    + HttpServlet의 service() 메서드는 템플릿 메서드 패턴으로 구현되어 있다. service() 메서드는 request의 type(HTTP Method: GET, POST, PUT, DELETE 등)에 따라 적절한 메서드(doGet, doPost, doPut, doDelete 등)를 호출한다.
    + 즉, 하위 클래스에서 doGet, doPost 등의 메서드를 오버라이드해두면 HttpServlet의 service() 메서드가 요청에 맞는 메서드(하위 클래스에서 오버라이드한 메서드)를 알아서 호출할 수 있게 되는 것이다.
  + 메서드가 return하면 해당 thread는 제거된다.

* destroy()
  + 한 번만 수행된다.
  + Web Application이 갱신되거나 WAS가 종료될 때 호출된다.
  + 역할: Servlet 객체를 메모리에서 제거

```JAVA
// `javax.servlet.http.HttpServlet`를 상속받은 Servlet 클래스
public class LoginServlet extends HttpServlet {
    // doPost()를 재정의 
    protected void doPost(HttpServletRequest request, HttpServletResponse response throws ServletException, IOException {
        // read form fields
        String username = request.getParameter("username");
        String password = request.getParameter("password");
         
        // get response writer
        PrintWriter writer = response.getWriter();

        /* 여기서 -> DB 접근 등 Business Logic 부분을 처리 */
         
        // build HTML code (view 생성 부분)
        String htmlResponse = "<html>";
        htmlResponse += "<h2>Your username is: " + username + "<br/>";      
        htmlResponse += "Your password is: " + password + "</h2>";    
        htmlResponse += "</html>";	
         
        // return response
        writer.println(htmlResponse);         
    }
}
```
#### WAS는 웹 브라우저로부터 요청을 받으면

    요청할 때 가지고 있는 정보를 HttpServletRequest객체를 생성하여 저장한다.
    웹 브라우저에게 응답을 보낼 때 사용하기 위하여 HttpServletResponse객체를 생성한다.
    생성된 HttpServletRequest, HttpServletResponse 객체를 Servlet에게 전달한다.

#### 개발자는 일반적으로 javax.servlet.http.HttpServlet를 상속받은 Servlet 클래스를 작성한다.

    HttpServletRequest request 파라미터를 통해 사용자가 입력한 form data를 읽는다.
    HttpServletResponse response 파라미터를 통해 출력/결과 Web Page를 생성한다.

#### 개발자는 Servlet 클래스에 doGet() 또는 doPost() 중 적어도 하나를 재정의하여 작성한다.

    protected doGet()(HttpServletRequest request, HttpServletResponse response){}
    protected doPost()(HttpServletRequest request, HttpServletResponse response){}

#### HttpServletRequest request 객체

    사용자가 HTML Form에 입력한 내용(username과 password)을 request 객체에서 받아온다.
        즉, HTTP 프로토콜의 Request 정보를 Servlet에게 전달
    헤더 정보, 파라미터, 쿠키, URI, URL, Body의 Stream 등을 읽어 들이는 메서드가 있다.
    getHeader(“원하는 헤더 이름”)
        이 메서드를 통해 원하는 헤더 정보를 확인할 수 있다.
    getParameter()
        이 메서드를 호출하여 form parameter 값을 가져온다.
            이런 parameter 값은 URL 또는 form의 input tag를 통해서 넘어올 수 있다.
        Ex.

        String name = request.getParameter("firstname");
        int year = Integer.parseInt (request.getParameter("year"));

    getParameterValues()
        form parameter가 두 번 이상 나타나고 여러 개의 값을 반환할 때 이 메서드를 호출한다. (Ex. checkbox)
        Ex.

        String languages[] = request.getParameterValues("language");

#### HttpServletResponse response 객체

    인자의 내용에 맞게 동적인 HTML 코드를 생성하여 response 객체에 담아 반환한다.
    getWriter() 메서드를 호출하여 PrintWriter 객체을 가져온 후 해당 객체에서 print, println 메서드를 실행한다.
    즉, form data를 처리한 결과를 Web Page에 생성(view 생성)하여 반환한다.
    Ex.

    response.setContentType("text/html"); 
    PrintWriter out = response.getWriter(); 
    out.println("<h2> Welcome </h2>");
### Servlet Concurrency
<img src="https://gmlwjd9405.github.io/images/web/servlet-concurrency.png"></img>
Java 서블릿 컨테이너 / 웹 서버는 일반적으로 멀티 쓰레드 환경이다.

    같은 Servlet에 대한 여러 개의 요청이 동시에 실행될 수 있어 runtime에 따라 결과가 달라질 수 있다.
    즉, Servlet은 메모리에 한 번 올라오고 멀티 쓰레드 환경에서 여러 thread는 하나의 Servlet을 공유하기 때문에 Concurrency Control(병행성 제어)가 필요하다.

Servlet의 service() 메서드 안의 변수

    정적 변수, 멤버 변수: 공유하는 지원이므로 상호배제가 필요
    지역 변수: thread마다 독립적으로 생성
### Servlet Annotation
<img src="https://gmlwjd9405.github.io/images/web/servlet-annotations.png"></img>
Servlet API 3.0은 javax.servlet.annotation 이라는 새로운 패키지를 도입했다.

    Tomcat 7 이상에서 사용 가능

Annotation은 Web Deployment Descriptor 파일(web.xml)의 XML 설정을 대체할 수 있다.

    설정 파일이 너무 길어지면 Annotation으로 변경한다.

Annotation Type

    @WebServlet : 서블릿 선언
    @WebInitParam : 초기화 매개 변수 지정
    예시

```JAVA
@WebServlet(value = "/Simple", 
initParams = {@WebInitParam(name="foo", value="Hello "), 
              @WebInitParam(name="bar", value=" World!")}) 
public class Simple extends HttpServlet { 
    protected void doGet(HttpServletRequest request, HttpServletResponse response throws ServletException, IOException {
	      response.setContentType("text/html"); 
	      PrintWriter out=response.getWriter(); 

	      out.print("<html><body>"); 
	      out.print("<h3>Hello Servlet</h3>"); 
	      out.println(getInitParameter("foo")); 
	      out.println(getInitParameter("bar")); 
	      out.print("</body></html>"); 
    } 
} 
```

    버전에 따른 Servlet 사용 방법
        Servlet 3.0 spec 이상
        web.xml 파일을 사용하지 않는다.
        자바 어노테이션(annotation)을 사용
        @WebServlet("/test")
            Servlet 3.0 spec 미만
        Servlet을 등록할 때 web.xml 파일에 직접 등록 ```xml
    SimpleServlet SimpleServlet exam.SimpleServlet SimpleServlet /test

   
        servlet-mapping에서 해당 url이 존재하지 않으면 404 응답 코드를 반환한다.
        servlet-mapping의 servlet-name과 servlet의 servlet-name을 동일하게 작성해야 해당 url에 맞는 Servlet을 찾을 수 있다.

관련된 Post
```
    JSP의 개념과 기본 문법에 대해 알고 싶으시면 JSP 란을 참고하시기 바랍니다.
    Web Server와 WAS의 개념과 차이에 대해 알고 싶으시면 Web Server VS WAS를 참고하시기 바랍니다.
    Web Application Structure과 web.xml 설정 내용, 
    역할 및 간단한 예시에 대해 알고 싶으시면 Web Application Structure 이해하기를 참고하시기 바랍니다.
```
References
```
        http://www.nextree.co.kr/p8428/
        https://1ambda.github.io/javascripts/rest-api-put-vs-post/
        http://finerss.tistory.com/11
        https://www.youtube.com/watch?v=aP4Lw3SfffQ&index=6&list=PLqaSEyuwXkSoeqnsxz0gYWZMihw519Kfr
        https://www.edwith.org/boostcourse-web/lecture/16688/
        https://www.edwith.org/boostcourse-web/lecture/16689/
```        
출처 : https://gmlwjd9405.github.io/2018/10/28/servlet.html
