# LANGUAGE 종류

## DML(Data Manipulation Language) : 데이터 조작 언어
- **INSERT : 컬럼에 데이터 추가**
  - `INSERT INTO 테이블명 VALUES(추가할데이터...)`
- **UPDATE : 컬럼 데이터 변경**
  - `UPDATE 테이블명 SET 컬럼명 = "변경할데이터" WHERE ...`
- **DELETE : 컬럼 데이터 삭제**
  - `DELETE 테이블명 WHERE ...`

### DML의 제약조건
- **PRIMARY KEY : 중복된 값 안되고 NULL값 안된다.**
- **UNIQUE : 중복된 값 안되고 고유의 값**
- **CHECK : 특정 조건을 설정해 그 조건을 항상 만족**
- **NOT NULL : NULL값이 올 수 없다.**
- **FORIEGN KEY : 외부값을 참조해서 그 값들 중 하나만 가질 수 있다.**

### 제약 생성,수정,삭제
**1. 생성**
   ```sql
   CREATE TABLE EMP
   (ENAME VARCHAR2(20) CONSTRAINT EMP_ENAME_PK PRIMARY KEY)
   ```
   CONSTRAINT EMP_ENAME_PK 부분은 제약명 생성 부분으로 생략가능<br>
**2. 수정**<br>
    `ALTER TABLE EMP MODIFY ENAME VARCHAR2(20) NOT NULL`<br>
**3. 삭제**<br>
    `ALTER TABLE EMP DROP CONSTARINT EMP_ENAME_PK`
## DDL(Date Definition Language) : 데이터 정의 언어 ( AUTO COMMIT )
- **CREATE : 테이블 생성**
  - `CREATE TABLE 테이블명 ( 컬럼명1 자료형(), 컬럼명2 자료형() ....)`

- **ALTER : 컬럼 변경**
  - 'ALTER TABLE 테이블명 ADD 컬럼명 자료형()' : 컬럼추가
  - `ALTER TABLE 테이블명 MODIFY 컬럼명 자료형()` : 자료형변경
  - `ALTER TABLE 테이블명 RENAME COLUMN 기존명 TO 바꿀명' : 컬럼명변경
  - `ALTER TABLE 테이블명 DROP COLUMN 컬럼명` : 컬럼삭제

- **DROP : 테이블 삭제**
  - `DROP TABLE 테이블명`

- **기타**
  - `RENAME 기존테이블명 TO 바꿀테이블명` : 테이블명 변경
  - `TRUNCATE TABLE 테이블명` : 테이블 내의 모든 데이터 삭제
  - `COMMENT ON 테이블명 IS '주석'` : 테이블 주석달기, 주석 보는법(DESC_USER_TAB_COMMENTS;)

## DCL(Data Control Language) : 데이터 제어 언어
- **GRANT : DB 사용자에게 특정 작업에 대한 수행 권한을 부여함**
  - `GRANT 권한 TO 사용자명` : 관리자 계정에서 권한을 부여할 수 있다.
  - `GRANT 권한 ON 오브젝트명 TO 사용자명` : 특정 오브젝트를 사용할 수 있는 권한 부여
- **REVOKE : DB 사용자에게 특정 작업에 대한 수행 권한을 회수함**'
  - `REVOKE 권한 FROM 사용자명` : 권한을 회수
  - `REVOKE 권한 ON 오브젝트명 FROM 사용자명` : 특정 오브젝트를 사용할 수 있는 권한 회수

### 권한 종류
- **CONNECT : 사용자에게 접속 권한 부여**
- **RESOURCE : 사용자에게 DML, DDL 권한 부여**

## TCL(Transaction Control Language)
- **COMMIT : DML이 잘 실행되었다면 내용을 저장**
- **ROLLBACK : 명령문이 잘못되어 DML을 취소해야 할때**



