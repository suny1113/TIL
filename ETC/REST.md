# REST ( Representational State Transfer )
- 다양한 웹 애플리케이션, 다른 서버 등에서 서로 통신할 수 있도록 통신 역할을 해주는 API
- HTTP URI를 통해 자원을 명시하고, HTTP Methond(GET, POST, PUT, DELETE)를 통해 해당 자원에 대한 CRUD 를 적용

# REST의 구성
### 1. 자원 (Resource)
- server에 존재하며, 접근할 대상이다.

### 2. 행위 (Verb)
- HTTP Method를 사용해 CRUD 작업을 한다.

|Method|설명|
|------|----|
|GET|조회|
|POST|생성|
|PUT|수정|
|DELETE|삭제|

### 3. 표현 (Representation of Resource)
- Client가 자원의 조작을 요청하면 server가 응답
- JSON 혹은 XML을 통해 데이터를 주고받는다.

# RESTful 이란
- REST의 기본 원칙을 잘 지킨 서비스 디자인을 RESTful하다고 표현한다.

# REST 활용
- 라이브러리 필요
- maven repository에서 Jackson Dataformat와 Jackson Core 라이브러리 추가
```xml
<dependency>
<groupId>com.fasterxml.jackson.core</groupId>
<artifactId>jackson-databind</artifactId>
<version>2.13.0</version>
</dependency>
	
<dependency>
<groupId>com.fasterxml.jackson.dataformat</groupId>
<version>2.13.0</version>
</dependency>
```

### 활용예시
```java
package kr.co.jhta.control;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.MediaType;
import org.springframework.web.bind.annotation.DeleteMapping;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.PutMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

import kr.co.jhta.dao.Dao;
import kr.co.jhta.dto.DeptDTO;

@RestController
@RequestMapping("/rest")
public class DeptController {

	@Autowired
	Dao dao;

	@GetMapping(value = "/test1", produces = "text/plain;charset=UTF-8")
	public String test() {
		return "Hello REST World!!";
	}

	// xml형태로 리턴
	@GetMapping(value = "/dept", produces = MediaType.APPLICATION_XML_VALUE)
	public List<DeptDTO> getAll() {
		return dao.getAll();
	}

	// JSON형태로 리턴
	@GetMapping(value = "/dept/{deptno}", produces = MediaType.APPLICATION_JSON_VALUE)
	public DeptDTO getOne(@PathVariable("deptno") int deptno) {
		return dao.getOne(deptno);
	}

	// 생성
	@PostMapping(value = "/dept/{deptno}", produces = MediaType.APPLICATION_JSON_VALUE)
	public DeptDTO insert(@PathVariable("deptno") int deptno, @RequestParam("dname") String dname,
			@RequestParam("loc") String loc) {
		DeptDTO dto = new DeptDTO(deptno, dname, loc);
		dao.insertOne(dto);
		return dto;
	}

	// 수정
	@PutMapping(value = "/dept/{deptno}")
	public void modify(@PathVariable("deptno") int deptno, @RequestParam("dname") String dname,
			@RequestParam("loc") String loc) {
		DeptDTO dto = new DeptDTO(deptno, dname, loc);
		dao.updateOne(dto);
	}
	
	// 삭제
	@DeleteMapping(value = "/dept/{deptno}")
	public void delete(@PathVariable("deptno") int deptno) {
		dao.deleteOne(deptno);
	}
}

```

- RestController 어노테이션을 사용하면 return이 view 를 반환하지 않고 서버에서 전송한 자원들의 목록을 반환한다.
- POST, PUT, DELETE 는 postman이나 Swagger Inspector와 같은 확장 프로그램을 통해서 테스트해본다.
