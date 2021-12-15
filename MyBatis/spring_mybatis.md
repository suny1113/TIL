# Spring에서 mybatis 설정
- spring에서는 객체를 직접 생성하지 않고, bean으로 객체를 생성한다(의존성 주입).
- mybatis도 마찬가지로 spring에서 사용하기 위해 필요한 객체를 bean에 등록해야 한다.
- Bean Configuration File에 Mybatis 설정정보를 등록하는법을 정리

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

<!-- Mybatis 설정 -->

<!-- db 설정정보를 읽는 객체 -->
<bean id="configurer" class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">
	<property name="location" value="db설정정보관련 파일 경로 입력" />
</bean>

<!-- db 연결정보 -->
<bean id="dataSoucre" class="org.apache.tomcat.dbcp.dbcp2.BasicDataSource">
	<property name="driverClassName" value="${driver}" />
	<property name="url" value="${url}" />
	<property name="username" value="${username}" />
	<property name="password" value="${password}" />
</bean>

<!-- factory 객체 생성 -->
<bean id="sqlSessionFactoryBean" class="org.mybatis.spring.SqlSessionFactoryBean">
	<property name="dataSource" ref="dataSoucre"/>
	<property name="configLocation" value="sqlMapConfig.xml파일의 경로" />
</bean>

<!-- SqlSessionTemplate -->
<bean id="sqlSessionTemplate" class="org.mybatis.spring.SqlSessionTemplate">
	<constructor-arg ref="sqlSessionFactoryBean" />	
</bean>


</beans>
```

### PropertyPlaceHolderConfigurer
- db의 설정정보를 읽는 객체
- db관련 정보를 저장한 db.properties 파일의 경로를 속성값으로 지정해준다.

### BasicDataSource
- db 연결정보를 읽는 객체

### SqlSessionFactoryBean
-  SqlSessionFactory 객체를 생성

### SqlSessionTemplate
- SqlSession을 구현해주고 코드에서 SqlSession을 대체하는 역할을 한다.
