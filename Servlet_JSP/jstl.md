# JSTL ( JSP Standard Tag Library )
- 스크립트릿을 사용할 필요 없이 JSP를 단순화 하는 많은 태그를 제공한다.
- EL과 함께 사용한다.
- 코드의 복잡성을 줄일 수 있고, 유지보수를 쉽게 할 수 있다.

## JSTL 라이브러리 종류
|라이브러리|접두어|기능|
|----|-|----|
|core|c| 자바언어와 유사한 언어를 사용, 사용흐름, 실행흐름 제어|
|XML|x| xml문서 처리기능 지원|
|국제화|fmt| 숫자, 날짜, 시간 포맷기능, 각종 나라 언어 지원|
|데이터베이스|sql|데이터베이스 데이터 crud기능 지원
|함수|fn|문자열 처리 함수 지원|

## JSTL 라이브러리 설치
1. http://tomcat.apache.org/ 방문
2. 왼쪽 download 항목의 Taglibs 클릭<br>
![jstl01](./images/jstl01.png)
3. jar 파일 링크 다운로드<br>
![jst102](./images/jstl02.png)
4. 이클립스 WEB_INF 하위폴더 lib에 jar파일 복사

