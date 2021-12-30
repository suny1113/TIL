# JPA (Java Persistence API)
- 자바의 ORM 기술의 표준
- 인터페이스의 모임이며 구현체가 없으며 사용하기 위해서는 ORM 프레임워크를 선택한다.
- 객체와 매핑을 해주기 위해 사용되는 프레임워크로 Hibernate를 주로 사용한다.

### ORM이란??
- Object-Relational Mapping
- 객체와 관계형 데이터베이스를 매핑한다는 뜻이며 객체는 객체대로, 데이터베이스는 데이터베이스대로 설계가 가능하다.

# JPA를 사용하는 이유
- 테이블에 변경된 컬럼이 있거나 SQL문을 모두 변경해야 하는 경우 등 반복적인 SQL문의 작업을 처리할 수 있어 유지보수가 용이하다.
- SQL문 중심이 아닌 객체 중심으로 개발할 수 있다.

# 동작방식
1. JPA 내부에서 JDBC API를 사용해 SQL을 호출해 DB와 통신한다.
2. 메서드로 데이터를 조작하며 객체간 관계를 바탕으로 SQL문을 자동으로 생성한다.

# JPA 활용하기

### 라이브러리 추가
- Maven Repository사이트에서 라이브러리 두개를 복사한 후 pom.xml에 추가한다.
```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-orm -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-orm</artifactId>
    <version>5.3.10</version>
</dependency>
<!-- https://mvnrepository.com/artifact/org.hibernate/hibernate-entitymanager -->
<dependency>
    <groupId>org.hibernate</groupId>
    <artifactId>hibernate-entitymanager</artifactId>
    <version>5.6.3.Final</version>
</dependency>


```
### 스프링 bean 설정파일 DB연결정보와 컨테이너 등록부분
```xml
<!-- 데이터베이스 연결정보 -->
	<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
		<property name="driverClassName" value="oracle.jdbc.driver.OracleDriver" />
		<property name="url" value="jdbc:oracle:thin:@localhost:1521:orcl" />
		<property name="username" value="scott" />
		<property name="password" value="tiger" />
	</bean>

	<!-- ORM 프레임워크 사용을 위한 로컬 컨테이너 등록 -->
	<bean id="" class="org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean">
		<property name="dataSource" ref="dataSource"/>
		<property name="persistenceXmlLocation" value="/META-INF/persistence.xml"/>
		<property name="jpaVendorAdapter">
			<bean class="org.springframework.orm.jpa.vendor.HibernateJpaVendorAdapter">
				<property name="generateDdl" value="true"></property>
			</bean>
		</property>
	</bean>
<!-- 트랜잭션 관리를 위한 설정 -->
	<bean id="txManager" class="org.springframework.orm.jpa.JpaTransactionManager">
		<property name="entityManagerFactory" ref="entityManagerFactory"/>
	</bean>
	
	<!-- 트랜잭션의 commit, rollback 담당 -->
	<tx:annotation-driven transaction-manager="txManager"/>
```

### META-INF 폴더하위에 persistence.xml 파일 생성
```xml
<?xml version="1.0" encoding="UTF-8"?>
<persistence xmlns="http://xmlns.jcp.org/xml/ns/persistence"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/persistence http://xmlns.jcp.org/xml/ns/persistence/persistence.xsd">

	<!-- hibernate 설정 -->
	
	<persistence-unit name="persistence-unit" transcation-type="RESOURCE_LOCAL">
		<provider>org.hibernate.ejb.HibernatePersistence</provider>
		<properties>
			<property name="hibernate.diarect" value="${hibernate.dialect}" />
			<property name="hibernate.hbm2ddl.auto" value="${hibernate.hbm2ddl.auto}" />
		</properties>
	</persistence-unit>

</persistence>
```

### 활용 코드
#### DTO 클래스
```java
package kr.co.jhta.dto;

import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.Table;

import lombok.AllArgsConstructor;
import lombok.Builder;
import lombok.Data;
import lombok.NoArgsConstructor;

@Entity
@Table(name = "department2") // department2 라는 테이블을 생성해 사용한다.
@Data
@Builder
@NoArgsConstructor
@AllArgsConstructor
public class DeptDTO {

	@Id // pk값
	@GeneratedValue(strategy = GenerationType.SEQUENCE) // 시퀀스를 이용
	private int deptno;
	@Column(length = 40, nullable = false) // false가 기본
	private String dname;
	private String loc;
}
```
- @Table 어노테이션으로 엔티티 명 지정해준다. (새로생길 테이블명)
- 변수에 어노테이션으로 pk, 시퀀스 등을 설정해줄 수 있다.

#### DAO 클래스
```java
package kr.co.jhta.dao;

import java.util.List;

import javax.persistence.EntityManager;
import javax.persistence.PersistenceContext;

import org.springframework.stereotype.Repository;
import org.springframework.transaction.annotation.Transactional;

import kr.co.jhta.dto.DeptDTO;

@Repository
@Transactional
public class DeptDAO implements Dao {

	// EntityManager 객체를 주입
	@PersistenceContext
	private EntityManager em;
	
	@Override
	public List<DeptDTO> getList() {
		return em.createQuery("select b from DeptDTO b order by b.deptno desc").getResultList();
	}

	@Override
	public DeptDTO getOne(int deptno) {
		return em.find(DeptDTO.class, deptno);
	}

	@Override
	public void insertData(DeptDTO dto) {
		em.persist(dto);
	}

	@Override
	public void updateData(DeptDTO dto) {
		em.merge(dto);
	}

	@Override
	public void deleteData(int deptno) {
		em.remove(em.find(DeptDTO.class, deptno));
		
	}

}
```
- @Transactional 어노테이션 추가
- @PersistenceContext어노테이션을 EntityManager에 등록

#### CRUD 관련 메서드
- list 얻어오기 : createQuery(쿼리문작성).getResultList();
- list중 1개의 data 가져오기 : entityManager.find()
- data 추가 : entityManager.persist()
- data 수정 : entityManager.merge()
- data 삭제 : entityManager.remove(entityManager.find());


#### JPA 기본문법
1. 대소문자 구분 : SELECT, FROM등과 같은 SQL 키워드는 대소문자 구분 x, 엔티티이름인 deptno 등은 대소문자 구분 o
2. @Entity 어노테이션에서 name값은 새로 생성되는 테이블의 이름이다. (생략하면 기본값)
3. 엔티티의 별칭을 필수적으로 명시해야 한다.
