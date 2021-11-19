# EL (Expression Language)
- jsp의 출력 문법을 대체하는 표현 언어이다.
- 기본문법은 ${표현식}
- 표현식에는 변수명, 속성명, 메소드 구조로 이루어져 있다.
- 정수, 실수, 문자열, 논리형, null이 올 수 있다.

## EL의 기본 객체
|객체|설명|
|---|----|
|pageContext| JSP의 기본 객체와 동일한 데이터 저장 영역에서 관리|
|pageScope| JSP의 pageContext 속성을 관리|
|requestScope| request 객체의 속성을 관리|
|sessionScope| session 객체의 속성을 관리|
|applicationScope| application의 속성을 관리|

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<%
	  pageContext.setAttribute("id1", "aaa");
	  request.setAttribute("id2", "bbb");
	  session.setAttribute("id3", "ccc");
	  application.setAttribute("id4", "ddd");
		
	%>
	
	<h3>pageContext : ${pageScope.id1}</h3>
	<h3>request : ${requestScope.id2}</h3>
	<h3>session : ${sessionScope.id3 }</h3>
	<h3>application : ${applicationScope.id4 }</h3>
  
  <!-- 위와 동일 (기본객체 생략가능) -->
  <h3>pageContext : ${id1}</h3>
	<h3>request : ${id2}</h3>
	<h3>session : ${id3 }</h3>
	<h3>application : ${id4 }</h3>
</body>
</html>
```

## EL의 매개변수 관련 객체
|객체|설명|
|---|----|
|param| 요청객체에 의해 전달받은 데이터를 관리|
|paramValues| 요청객체의 의해 전달받은 데이터들을 관리|

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<%
		Object n1 = request.getParameter("no1");
		Object[] n2 = request.getParameterValues("no2");
	
	%>
  
	${param.no1}
  
  ${paramValues.no2[0]}
  ${paramValues.no2[1]}
  ${paramValues.no2[2]}
  ${paramValues.no2[3]}
  ...

</body>
</html>
```

## EL의 연산자
###수치연산자
```jsp
${"1"+1} // 2
${"1"+="1"} //11
${null+1} // 0+1 = 1
```

### 비교연산자
```jsp
${"2" > 1} // true
${empty null} // true
${null == 0} // false
${null eq 0} // false
```
