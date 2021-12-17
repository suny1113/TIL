# Logging이란?
- 개발 중이나 완료 후 발생할 수 있는 오류들을 디버깅하거나 운영 중인 프로그램을 모니터링 하기 위해 필요한 정보를 기록하는 것.
- 애플리케이션 실행에 대한 추적을 기록하기 위해 콘솔이나 파일 등으로 작성하는 것.

# SLF4J (Simple Logging  Facade for Java)
- Logging 모듈들의 추상체
- 추상 로깅 프레임워크이기 때문에 구현체의 종류에 상관없이 일관된 로깅을 할 수 있다. (인터페이스와 비슷한 개념)
- 실제 코드에서 slf4j의 interface코드를 사용하고, 실제 로깅을 하는 구현체는 라이브러리에서 구현됨.

# SLF4J binding with logback
- pom.xml에 slf4j-api 라이브러리와 logback-classic 라이브러리 추가

```xml
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-context</artifactId>
  <version>5.3.10</version>
  <exclusions>
    <exclusion>
     <groupId>commons-logging</groupId>
     <artifactId>commons-logging</artifactId>
   </exclusion>
  </exclusions>
</dependency>

<dependency>
  <groupId>org.slf4j</groupId>
  <artifactId>jcl-over-slf4j</artifactId>
  <version>1.7.7</version>
  <scope>runtime</scope>
</dependency>

<dependency>
  <groupId>ch.qos.logback</groupId>
  <artifactId>logback-classic</artifactId>
  <version>1.2.3</version>
</dependency>
```

# logback.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>

<configuration> <!-- 로깅 기록을 파일로 저장 -->
	<appender name="FILE" class="ch.qos.logback.core.FileAppender">
		<file>c:\log\log1.log</file> <!-- log폴더에 log1.log라는 파일명으로 로그 기록 -->
		<encoder>
			<pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
		</encoder>
	</appender>
	              <!-- 로깅 기록을 콘솔에 띄움 -->
	<appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
		<encoder>
			<pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
		</encoder>
	</appender>
	
	<logger name="kr.co.jhta.web.controller" level="info" additivity="false">
		<appender-ref ref="FILE" />
		<appender-ref ref="STDOUT" />
	</logger>
	
	<root level="warn">
		<appender-ref ref="FILE"/>
		<appender-ref ref="STDOUT"/>
	</root>
</configuration>
```

# 실제 코드에 적용
```java

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

import lombok.extern.slf4j.Slf4j;

@Slf4j // Slf4j 어노테이션 사용
@Controller
public class HelloController {

//	private static final Logger LOGGER = LoggerFactory.getLogger(HelloController.class);
	
	@RequestMapping("/hello")
	public String Hello() {
		log.info("hello.jsp로 이동중");
		return "hello";
	}
}

```
