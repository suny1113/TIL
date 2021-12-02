# Spring Bean이란?
Spring Ioc 컨테이너가 관리하는 자바의 객체를 Bean이라 한다.

여기서 Ioc란 (Inversion of Control) 무엇일까?

Ioc란 제어의 역전, 기존의 사용자가 객체를 직접 생성하고 호출하는 등의 작업을 하였다면,<br> Ioc의 개념은 이러한 객체를 생성하는 작업을 직접 하지 않고 스프링에게 맡긴다는 것이다.

즉 Bean(빈)이란, 객체를 직접 new 하지 않고, Spring에 의해서 생성되고 관리되는 자바 객체를 말한다.

## Spring Bean을 Ioc 컨테이너에 등록하기

몇가지 방법이 있지만 오늘 배운 xml파일을 통해 Ioc 컨테이너에 Bean을 등록하는 방법을 정리해보겠다.

### JDBC에서 DAO 객체를 Bean에 등록하기

1. Spring Bean Configuration File 생성
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">
        
</beans>
```
2. Maven project로 전환 후 pom.xml파일에 spring-context 등록 후 JDBC에 필요한 라이브러리 등록(lombok, ojdbc6)
- Maven Repository 사이트에서 필요한 라이브러리를 얻을 수 있다.
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>spring_study</groupId>
	<artifactId>spring_study</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<dependencies>
		<!-- https://mvnrepository.com/artifact/org.springframework/spring-context -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context</artifactId>
			<version>5.3.10</version>
		</dependency>
		<!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
		<dependency>
			<groupId>org.projectlombok</groupId>
			<artifactId>lombok</artifactId>
			<version>1.18.20</version>
			<scope>provided</scope>
		</dependency>
		<!-- https://mvnrepository.com/artifact/com.oracle.database.jdbc/ojdbc6 -->
		<dependency>
			<groupId>com.oracle.database.jdbc</groupId>
			<artifactId>ojdbc6</artifactId>
			<version>11.2.0.4</version>
		</dependency>
	</dependencies>
	<build>
		<sourceDirectory>src</sourceDirectory>
		<plugins>
			<plugin>
				<artifactId>maven-compiler-plugin</artifactId>
				<version>3.8.1</version>
				<configuration>
					<source>1.8</source>
					<target>1.8</target>
				</configuration>
			</plugin>
		</plugins>
	</build>
</project>
```

3. DTO 클래스와 DAO클래스 작성
```java
// DTO
import lombok.AllArgsConstructor;
import lombok.Getter;
import lombok.NoArgsConstructor;
import lombok.Setter;

@Setter
@Getter
@NoArgsConstructor
@AllArgsConstructor

public class DeptDTO {
	private int deptno;
	private String dname;
	private String loc;
}

```

```java
// DAO
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.ArrayList;
import java.util.List;

public class DeptDAO {
	String driver = "oracle.jdbc.driver.OracleDriver";
	String url = "jdbc:oracle:thin:@localhost:1521:orcl";
	String user = "db유저명";
	String password = "비밀번호";
	Connection conn;
	
	public Connection connect() {
		try {
			Class.forName(driver);
			conn = DriverManager.getConnection(url, user, password);
		} catch (ClassNotFoundException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		return conn;
	}
	
	public List<DeptDTO> selectAll(){
		ArrayList<DeptDTO> list = new ArrayList<DeptDTO>();
		try {
			PreparedStatement pstmt = conn.prepareStatement("SELECT * FROM dept ");
			ResultSet rs = pstmt.executeQuery();
			while(rs.next()) {
				list.add(new DeptDTO(rs.getInt("deptno"),rs.getString("dname"), rs.getString("loc")));
			}
		} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		return list;
	}
}
```

4. DAO클래스를 Bean에 등록
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

	<bean id="dao" class="spring_study.DeptDAO"></bean>
</beans>

```

5. main 메서드에서 db연동 테스트해보기
```java
import java.util.List;

import org.springframework.beans.factory.BeanFactory;
import org.springframework.beans.factory.xml.XmlBeanFactory;
import org.springframework.core.io.FileSystemResource;

public class TestMain {

	public static void main(String[] args) {
		BeanFactory factory = new XmlBeanFactory(new FileSystemResource("src/app.xml"));
		
		DeptDAO dao = factory.getBean("dao", DeptDAO.class);
		
		dao.connect();
		List<DeptDTO> list = dao.selectAll();
		
		for(DeptDTO dto : list) {
			System.out.println(dto.getDeptno()+" : "+dto.getDname()+" : "+dto.getLoc());
		}

	}

}
// 결과값

//50 : IT : SEOUL
//3 : 영업 : 서울
//4 : 영업 : 서울
//7 : 영업 : 하와이
//10 : ACCOUNTING : NEW YORK
//20 : RESEARCH : DALLAS
//30 : SALES : CHICAGO
//40 : OPERATIONS : BOSTON
```
