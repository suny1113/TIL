# ORACLE의 select문

## 작성순서

```sql
SELECT
FROM
WHERE
GROUP BY
HAVING
ORDER BY
```
### SELECT절
- 사칙연산 가능
- 별칭지정 가능 ""
- 문자열 결합 ||
- 함수 사용가능

### FROM절
- 조회할 컬럼들이 속해있는 테이블로부터 데이터를 가져온다.
- 별칭지정 가능 ex) `FROM EMP E`

### WHERE절
- 비교연산자 
- BETWEEN, OR
- IN (OR 여러번쓸때)
- IS NULL
- LIKE (~의 문자를 가진것들)

### GROUP BY절
- 그룹별 건수나 합계를 구할때
- 복수행 함수 출력할때 사용
- 특정 조건으로 세부적인 그룹화<br>
ex) 부서별 임금의 평균
```sql
SELECT AVG(SAL)
FROM EMP
GROUP BY DEPTNO;
```

### HAVING절
- 그룹절에 조건을 추가할때
ex) 1000만원이 넘는 부서별 임금의 평균
```sql
SELECT AVG(SAL)
FROM EMP
GROUP BY DEPTNO
HAVING AVG(SAL) >= 1000;
```

### ORDER BY절
- ASC는 오름차순, DESC는 내림차순
- default값은 오름차순
