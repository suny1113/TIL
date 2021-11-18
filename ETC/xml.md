# XML ( eXtensive Markup Language )
- W3C의 웹 표준언어로 개발되었으나 현재는 환경설정과 UI에 사용되는 언어이다.

## XML 구조
- 선언부   `<?xml version="1.0" encoding="UTF-8" ?>`
- 문서구조 `<!DOCTYPE 루트엘리먼트명 SYSTEM/PUBLIC "dtd파일의 경로와 파일명" >`


## XML의 태그
1. 시작태그와 종료태그가 있어야 한다.
2. 모든 element명은 대소문자를 구분한다.
3. element 내용을 빈 요소로 둘 수 있다.
4. 최상위 root element는 1개만 허용한다.
5. element명은 문자열'xml'로 시작할 수 없다.
6. element명은 반드시 문자 또는 _로 시작해야 한다.
7. element명은 공백을 사용할 수 없다.
8. 예약어를 사용할 수 없다.

## DTD 요소와 속성 선언
- `<!ELEMENT 요소이름 (요소내용) >`
- `<!ATTLIST 요소이름 속성이름 속성타입 속성값 >`

### 속성값 정의
|속성값|설명|
|----|---|
|값|속성값이 명시되지 않을경우 사용할 기본값|
|#REQUIRED|반드시 명시되어야 할 속성값|
|#IMPLED|명시되어도 되고, 명시하지 않아도 됨|
|#FIXED|명시된 값으로 고정됨|

## 예시
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE department [
  <!ELEMENT department (dept)+ >
  <!ELEMENT dept (deptno, dname, loc) >
  <!ELEMENT deptno (#PCDATA) >
  <!ELEMENT dname (#PCDATA) >
  <!ELEMENT loc (#PCDATA) >
]>

<department>
   <dept>
        <deptno>10</deptno>
        <dname>
          ACCOUNT
          &lt;&lt;&lt; ACCOUNT &gt;&gt;&gt; <!-- <<<ACCOUNT>>> 와 같다.-->
        </dname>
        <loc>DALLAS</loc>
   </dept>
  
   <dept>
        <deptno>20</deptno>
        <dname>
          RESEARCH
          <![CDATA[
          파싱하지 않는 문자열  
          ]]>
        </dname>
        <loc>DALLAS</loc>
   </dept>
  
   <dept>
        <deptno>30</deptno>
        <dname>SALES</dname>
        <loc>CHICAGO</loc>
   </dept>
</department>
```

## PCDATA와 CDATA란?
- PCDATA란 XML 파서에 의해 분석될 문자 데이터를 의미한다.
- XML ELEMENT 요소의 시작태그 종료태그 사이의 위치한 텍스트가 PCDATA이다.
- CDATA란 XML 파서가 분석하지 않는 문자 데이터를 의미한다.
- XML 속성의 속성값으로 CDATA만이 올 수 있다.

## ELEMENT들의 사용빈도
- '+' : 1개 이상
- '*' : 0개 이상
- '?' : 0개 또는 1개
- 'A|B' : A 또는 B 둘 중 하나만 가능
- '()' : 요소를 그룹으로 선언해서 사용

