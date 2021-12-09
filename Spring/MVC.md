# MVC 패턴
- 디자인 패턴으로 비즈니스 처리 로직과 UI를 분리하여 서로 영향없이 개발하는 방법

## Model
- 뷰와 컨트롤러 사이에서 데이터를 주고받을 수 있게 해주는 객체, 내부 비즈니스 로직 처리
## View
- 모델과 컨트롤러가 보여줄려고 하는 것들을 화면에 출력
## Controller
- 화면에서 사용자의 요청을 받아 처리되는 부분 구현, 요청 내용을 분석해 Model과 View에 업데이트 요청을 함

![mvc01](./images/mvc01.png)

## MVC 패턴 적용을 위한 web.xml 설정과 Bean Configuration file 설정
- web.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns="http://xmlns.jcp.org/xml/ns/javaee"
	xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
	version="4.0">
	<display-name>spring_web03</display-name>

	<servlet>
    <!-- 서블릿명은 마음대로 지정 가능 -->
		<servlet-name>dispatcher</servlet-name>
    <!-- DispatcherServlet의 경로 -->
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
	</servlet>

	<servlet-mapping>
		<servlet-name>dispatcher</servlet-name>
    <!-- 뒤에 .do로 끝나는 모든 패턴에 해당됨 -->
		<url-pattern>*.do</url-pattern>
	</servlet-mapping>

	<welcome-file-list>
		<welcome-file>index.html</welcome-file>
		<welcome-file>index.htm</welcome-file>
		<welcome-file>index.jsp</welcome-file>
		<welcome-file>default.html</welcome-file>
		<welcome-file>default.htm</welcome-file>
		<welcome-file>default.jsp</welcome-file>
	</welcome-file-list>
</web-app>
```

- Bean Configuration file명은 `서블릿명-servlet.xml` 로 작성해주어야 한다.
- dispatcher-servlet.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

<!-- Handler Mapping -->
<bean id="beanNameUrlHandlerMapping" class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"></bean>
<!-- Controller 등록 -->
<bean id="/해당컨트롤러로이동.do" class="컨트롤러 클래스경로"></bean>
<!-- Bean 등록 -->

<!-- viewResolver -->
<bean id="internalResourceViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
  <!--접두사가 /인 모든 경우 -->
	<property name="prefix" value="/" />
  <!--접미사가 .jsp로 끝나는 모든 경우 -->
	<property name="suffix" value=".jsp" />
	
</bean>
</beans>

```
