# Spring Security
- 스프링 기반의 애플리케이션 보안을 담당하는 스프링 하위 프레임워크
- 보안 관련해서 체계적인 옵션을 제공해 주기 때문에 일일이 보안 로직을 작성하지 않아도 된다.
- filter와 intercepter로 동작하며 filter 는 web.xml 설정파일에, intercepter는 security-context.xml 파일에 설정한다.

# Security 설정
## pom.xml

```xml
                <dependency>
		    <groupId>org.springframework.security</groupId>
		    <artifactId>spring-security-core</artifactId>
		    <version>5.0.6.RELEASE</version>
		</dependency>
		<!-- https://mvnrepository.com/artifact/org.springframework.security/spring-security-config -->
		<dependency>
		    <groupId>org.springframework.security</groupId>
		    <artifactId>spring-security-config</artifactId>
		    <version>5.0.6.RELEASE</version>
		</dependency>
		
		<!-- https://mvnrepository.com/artifact/org.springframework.security/spring-security-web -->
		<dependency>
		    <groupId>org.springframework.security</groupId>
		    <artifactId>spring-security-web</artifactId>
		    <version>5.0.6.RELEASE</version>
		</dependency>
		<!-- https://mvnrepository.com/artifact/org.springframework.security/spring-security-taglibs -->
		<dependency>
		    <groupId>org.springframework.security</groupId>
		    <artifactId>spring-security-taglibs</artifactId>
		    <version>5.0.6.RELEASE</version>
		</dependency>
```
- core, config, web, taglibs 총 4개의 라이브러리가 필요하다.

## web.xml

```xml
        <filter>
		<filter-name>springSecurityFilterChain</filter-name>
		<filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
	</filter>
	
	<filter-mapping>
		<filter-name>springSecurityFilterChain</filter-name>
		<url-pattern>/*</url-pattern>
	</filter-mapping>
```

## security-context
```xml
<!-- 시큐리티의 시작지점 지정 -->
<security:http>
  
</security:http>

<!-- 스프링 시큐리티가 동작하려면 AuthenticationManager 라는 인터페이스가 필요함 -->
<security:authentication-manager>
  
</security:authentication-manager>
```
- intercepter 등록 이전에 초기설정을 해야 실행이 된다.

# 인증과 인가
- 인증은 로그인을 하는 행위, 인가는 권한을 부여받는것을 의미한다.

## security-context intercepter 설정
```xml
          <security:http auto-config="true" use-expressions="true">
		<security:intercept-url pattern="/all" access="permitAll"/>
		<security:intercept-url pattern="/user" access="hasRole('ROLE_USER')"/>
		<security:intercept-url	pattern="/admin" access="hasRole('ROLE_ADMIN')"/>
	
		<security:csrf disabled="true"/>
	
		<!-- 커스텀 로그인 -->
		<security:form-login login-page="/cLogin" />
		
		<!-- 커스텀 로그아웃 -->
		<security:logout logout-url="/cLogout" 
							delete-cookies="JSESSIONID" 
							invalidate-session="true"
							logout-success-url="/"
		 />
		
		<!-- 접근이 거부되었을때 보여줄 에러페이지 -->
		<security:access-denied-handler error-page="/error"/>
	
	</security:http>

<security:authentication-manager>
		<security:authentication-provider>
			<security:user-service>
				<security:user name="user" authorities="ROLE_USER" password="{noop}123"/>
				<security:user name="admin" authorities="ROLE_ADMIN" password="{noop}123"/>
			</security:user-service >
		</security:authentication-provider>
	</security:authentication-manager>
```
- pattern은 url경로를 뜻하고, access는 접근 제한을 설정한다.
- ex) /all 매핑을 받는 경우 권한은 permitAll 이다.

|권한|설명|
|----|----|
|isAuthenticated() | 인증받은 사용자만 접근가능|
|hasRole|특정 권한이 부여된 사용자만 접근가능|
|permitAll| 모든 사용자가 접근 가능|

- 스프링 시큐리티는 자체적으로 로그인 페이지를 제공한다. 
- 기본 로그인 페이지를 사용하지 않기 위해 커스텀 로그인페이지를 등록한다.

## 인증을 받아야 하는 페이지에 접근한 경우 (접근순서)
1. 먼저 커스텀 로그인 페이지 매핑 "/cLogin"으로 이동한다.
2. /cLogin 매핑을 받는 컨트롤러로 이동한다.
```java
@GetMapping("/cLogin")
	public String customLogin(String error, String logout, Model model) {
		if(error != null) {
			model.addAttribute("msg", "로그인 에러");
		}else if(logout != null) {
			model.addAttribute("msg", "로그아웃");
		}
		
		return "common/cLogin";
	}
```
3. 커스텀 로그인 페이지로 이동한다.
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>

	<c:out value="${error }"></c:out>
	<c:out value="${logout }"></c:out>

	<form action="<c:url value="/login" />" method="POST">
		아이디<input type="text" name="username" id="" /><br>
		비밀번호 <input type="password" name="password" id="" /><br>
		<input type="submit" value="로그인" />
		<input type="hidden" name="${_crsf.parameterName }" value="${_crsf.token }" />
	</form>
</body>
</html>
```
- form태그의 url은 /login, method는 post 방식, id의 name값은 username, password의 name값은 password, hidden태그로 crsf 지정.

4. 로그인에 성공하면 이전에 요청했었던 페이지로 이동한다.

## 로그아웃
1. 커스텀 로그아웃에 지정한 매핑으로 이동한다.
2. 매핑을 컨트롤러가 받는다.
```java
@GetMapping("/cLogout")
	public String logout() {
		return "common/cLogout";
	}
```

3. 커스텀 로그아웃 페이지로 이동하고 로그아웃을 실시한다.
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	
	<form action="/cLogout" method="post">
		<input type="hidden" name="${_crsf.parameterName }" value="${_crsf.token }" />
		<button>로그아웃</button>
	</form>
	
</body>
</html>
```
