# Servlet이란?

서버에서 웹 페이지 등을 동적으로 생성하거나 데이터를 처리하기 위해 자바로 작성된 프로그램, 확장자는 .java

## Servlet의 특징

- 자바코드로 구현해야 하고 자바코드 안에 HTML 태그를 문자열로 처리해야 한다.
- 클라이언트의 요청을 동적으로 처리하고 그 결과를 다시 클라이언트에게 전송한다.
- HTML 코드 변경시 컴파일을 다시 해야한다.
	
## Servlet의 실행

1. 먼저 클라이언트가 데이터를 요청하면 요청 정보가 Servlet Container로 전송된다.

2. Servlet Container는 클라이언트의 요청을 처리해주고 응답할 수 있는 환경을 제공해주고, Tomcat이 대표적이다.

3. 클라이언트의 요청을 받은 후 자바 클래스 파일에서 HttpServlet 클래스를 상속받는다.

4. 상속을 받으면 요청을 처리하는 Request객체와 Response객체를 매개변수로 받는 doGet() 또는 doPost() 메서드를 호출한다.

5. 클라이언트의 get 또는 post 호출 방식에 맞춰 doGet(), 또는 doPost() 메서드를 오버라이딩한다.

6. doGet() 또는 doPost() 메서드 내의 html 코드를 삽입하여 동적 페이지를 생성한 후 요청에 응답한다.

## Servlet 작성순서

	1. HttpServlet 클래스를 상속받는다.
	2. 호출 방식에 맞춰 doGet(), 또는 doPost() 메서드를 오버라이딩한다.
	3. 메서드 내의 HTML코드를 삽입하기 위해 출력을 위한 객체 PrintWriter를 생성한다.
	4. 어노테이션을 이용해 서블릿을 매핑해준다. @WebServlet("/서블릿 매핑 이름")

```java
import jakarta.servlet.http.HttpServlet; // tomcat10버전, javax 패키지 -> jakarta 패키지로 변경
@WebServlet("/매핑")
public class Servlet01 extends HttpServlet {
@Override
	protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		resp.setCharacterEncoding("utf-8");
		
		PrintWriter out = resp.getWriter();
		out.println("<html>");
		out.println("<head>");
		out.println("<meta charset='utf-8'>");
		out.println("<title>Servlet Test</title>");
		out.println("<head>");
		out.println("<body>");
		out.println("작성할내용...");
		out.println("</body>");
		out.println("</html>");
	}
}
```
## Servlet의 라이프사이클
요청이오면 Servlet 클래스가 로딩되고, Servlet객체가 생성됨.
- init() : 최초 한번만 실행
- service() : 호풀 될때마다 실행
- destroy() : 서버 종료 또는 재가동될때 실행
```java
@Override
	public void init() throws ServletException {
		// 최초 한번만 실행
	}
	
	@Override
	public void service(ServletRequest arg0, ServletResponse arg1) throws ServletException, IOException {
		// 호출 될때마다 실행
	}
	
	@Override
	public void destroy() {
		// 서버 종료 또는 재가동될때 실행
	}
```


# JSP란?

위의 서블릿의 코드를 보면 
```java
out.println("<html>");
out.println("<head>");
out.println("<meta charset='utf-8'>");
out.println("<title>Servlet Test</title>");
out.println("<head>");
out.println("<body>");
out.println("작성할내용...");
out.println("</body>");
out.println("</html>");
```
일일이 out.println 메서드를 이용하여 HTML 코드를 삽입해야 하기 때문에 직관적이지 않고 작성하기에도 불편하다.

이러한 불편함을 해결하기 위해 JSP가 등장하였다.

JSP의 특징을 알아보자.

## JSP의 특징
- 자바코드 내에 HTML을 작성하기 어렵고 불편했기 때문에 Servlet의 단점을 보완해서 만들어진 기술이다.
- 서블릿이나 JSP나 근본적으로 같은 기술이다.
- HTML코드 내의 스크립트릿 <% %> 안에 자바코드를 작성한다.
- 표현식 <%= 자바코드 실행결과값출력 %> 을 사용해 결과값을 출력할 수 있다.
- 변수 선언부 <%! 변수선언 %> 변수를 선언할 수 있다.(선언위치 상관없다.)
	
## JSP의 실행

1. jsp파일을 실행하게 되면 WAS에 의해 내부적으로 Servlet 파일로 변환된다.
2. 변환된 Servlet파일로 처리하고 다시 클라이언트에게 응답한다.

## JSP의 내장객체
JSP 내부적으로 선언되어 있는 객체가 있는데 따로 선언할 필요 없이 사용이 가능하다.

내부 객체는 JSP 파일이 Servlet 파일로 전환될 때 생성된다.

내부 객체의 종류
- request, response : 클라이언트의 요청(request)처리, 처리 후 클라이언트로 응답(response)
- pageContext : 현재 jsp 페이지의 컨텍스트 정보를 나타내는 객체
- session : 웹 브라우저의 정보를 유지하기 위한 세션정보를 저장하는 객체
- application : 웹 application 컨텍스트 정보를 저장하고 있는 객체
	

