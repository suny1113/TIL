# VIEW란?
- 가상 테이블이며 원본 테이블에 가서 데이터를 불러온다.

## 특징
- 저장장소를 거의 사용하지 않아서 메모리 절약
- 논리적으로 테이블과 같은 역할
- VIEW에 DML작업을 실시하면 실제 테이블에 그대로 반영, ComplexView에서는 일부의 경우만 가능.

## VIEW를 사용하는 이유
- 원본 테이블에서 사용자에게 제공해야할 정보만 따로 뽑아낼 수 있기 때문에 사용자에게 편의성을 제공한다.
- 노출되어서는 안되는 중요한 데이터를 제외시켜 보안성을 강화시킬 수 있다.
- JOIN을 자주 해야하는 경우 VIEW를 생성해놓고 필요할때마다 조회하면 편리하다.

## SimpleView와 ComplexView
- SimpleView는 JOIN조건이 들어가지 않는 1개의 테이블로 만든 간단한 VIEW이다.
- ComplexView는 여러개의 테이블이 JOIN되어 생성된다.

## InlineView
- 서브쿼리를 FROM절 안에서 테이블처럼 사용한다.
- 따라서 VIEW를 FROM절 안에서 쓴다고 생각하면 된다.

## VIEW의 CRUD

### VIEW 생성
- CREATE OR REPLACE VIEW 뷰명
```SQL
CREATE OR REPLACE VIEW VIEW_EMP
AS SELECT EMPNO, ENAME, SAL
FROM EMP
WHERE DEPTNO = 10;
```
-> 부서번호가 10번인 사원의 사번, 이름, 임금을 VIEW로 만들기

### VIEW 조회
```SQL
SELECT TEXT
FROM USER_VIEWS
WHERE VIEW_NAME = VIEW_EMP;
```

### VIEW 삭제
```SQL
DROP VIEW VIEW_EMP;
```

# INDEX란?
- 데이터 양이 많을 때 검색속도를 향상시키기 위해 사용
- SQL문에 영향을 주지 않는다.
- 해당 컬럼의 INDEX를 찾아가 ROWID를 보고 테이블에 접근한다.
- ROWID를 이용해 가장 빠르게 데이터를 검색할 수 있다.

## INDEX를 사용하는 경우
- 자주 검색되는 컬럼
- JOIN 조건으로 자주 사용되는 컬럼

## INDEX 생성, 삭제

### 생성
```SQL
CREATE INDEX IDX_EMP_EMPNO
ON EMP(EMPNO);
```
-> EMP테이블의 EMPNO컬럼에 INDEX 생성

### 삭제
```SQL
DROP INDEX IDX_EMP_EMPNO;
```
-> PK와 UK가 지정된 INDEX는 삭제가 불가능하기 때문에 제약조건을 먼저 삭제한 후 INDEX를 삭제한다.
