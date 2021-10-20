# JSP파일에서 DB와 연동하기

Java 프로그램에서 SQL문을 실행하여 데이터를 관리하기 위한 Java API이다.

데이터와 관련된 변수를 선언하는 VO (Value Object) 클래스와<br>
SQL문을 작성해 데이터에 접근할 수 있는 코드를 모아놓은 DAO (Data Access Object)클래스를 작성하여 사용한다.

<h3>⦿다음부터 작성할 예제 코드는 부서 테이블의 부서번호(deptno), 부서명(dname), 위치(loc) 컬럼을 이용할 것이다.<br>
(DB는 Oracle 사용)<h3>

## VO(Value Object)

데이터와 관련된 변수들을 선언해놓은 클래스이다.

주로 변수, 기본생성자, 매개변수있는 생성자, getter, setter 메서드를 작성한다.

```java
public class DeptVO {
	private int deptno;
	private String dname;
	private String loc;
	
	public DeptVO() {}

	public DeptVO(int deptno, String dname, String loc) {
		super();
		this.deptno = deptno;
		this.dname = dname;
		this.loc = loc;
	}

	public int getDeptno() {
		return deptno;
	}

	public void setDeptno(int deptno) {
		this.deptno = deptno;
	}

	public String getDname() {
		return dname;
	}

	public void setDname(String dname) {
		this.dname = dname;
	}

	public String getLoc() {
		return loc;
	}

	public void setLoc(String loc) {
		this.loc = loc;
	}
}  
```

멤버변수는 외부에서 접근이 불가능하게 private제어자로 선언하고, setter 메서드와 getter 메서드를 이용해 변수에 접근해야 한다.

## DAO(Data Access Object)
  
DB와 연결을 하고 SQL문을 작성해 데이터를 가져오는 코드를 작성하는 클래스이다.
  
## 작성 순서
1. Class.forName(oracle.jdbc.driver.OracleDriver); // 드라이버의 로딩 여부를 확인한다.
2. Connection conn = DriverManager.getConnection(JDBC url, 사용자계정이름, 비밀번호); // 로딩 후 Connection 객체 생성해 DB와 연결한다.
3. SQL문 작성, StringBuffer를 이용해 문자열을 이어 붙이는 방식으로 작성한다. (SQL문이 길어질 경우를 대비해서)
```java
  StringBuffer sb = new StringBuffer();
  sb.setLength(0); // 다른 SQL문과 겹칠수 있기 때문에 항상 문자열의 길이를 0으로 초기화시켜주자.
  sb.append("SELECT deptno, dname, loc "); // 절이 끝날때마다 마지막에 띄어쓰기 항상 해주자 ( 문자열을 잇는 방식이기 때문에 공백이 없으면 SQL문 오류가 난다. )
  sb.append("FROM dept "); // SELECT문은 모든 컬럼을 조회할때 *를 사용할수 있는데 Java에서는 유지보수를 위해 왠만하면 모든 컬럼명을 적어주자.
```
4. PrepareStatement ps = conn.prepareStatement(sb.toString()); // SQL문을 실행한다.
5. ResultSet rs = ps.executeQuery(); // 결과값을 가져온다.

- 위의 순서를 모두 정리한 예제를 작성
- 모든 데이터를 조회 후 ArrayList 객체 안에 DeptVo 데이터값들을 저장한다.
  
```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.ArrayList;

import vo.DeptVO;

public class DeptDAO {
	
	String driver = "oracle:jdbc:driver:OracleDriver";
	String url = "jdbc:oracle:thin:@localhost:1521:orcl";
	Connection conn;
	PreparedStatement ps;
	ResultSet rs;
	StringBuffer sb = new StringBuffer();
	
	public DeptDAO() {
		try {
			Class.forName(driver);
			conn = DriverManager.getConnection(url, "사용자계정아이디", "비밀번호");
		} catch (ClassNotFoundException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}
	
	public ArrayList<DeptVO> selectAll() {
		sb.setLength(0);
		sb.append("SELECT deptno, dname, loc ");
		sb.append("FROM dept ");
		ArrayList<DeptVO> list = new ArrayList<DeptVO>();
		
		try {
			ps = conn.prepareStatement(sb.toString());
			
			rs = ps.executeQuery();
			while(rs.next()) {
				int deptno = rs.getInt("deptno");
				String dname = rs.getString("dname");
				String loc = rs.getString("loc");
				list.add(new DeptVO(deptno, dname, loc));
			}
		} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		return list;
	}
}
```
