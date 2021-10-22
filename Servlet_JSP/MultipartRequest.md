# MultipartRequest 클래스

- 파일 업로드를 직접 담당하는 클래스이다.
- 파일의 업로드를 담당하는 생성자 및 메서드를 포함하고 있다.

### 생성자
```java
MultipartRequest(javax.servlet.http.HttpServletRequest request,
                  java.lang.String saveDirectory,
                  int maxPostSize,
                  java.lang.String encoding,
                  FileRenamePolicy policy)
``` 

|인자|설명|
|---|---|
|request|MultipartRequest 클래스와 연결된 request 객체|
|saveDirectory|서버 컴퓨터에 파일이 실질적으로 저장될 경로|
|maxPostSize|한번에 업로드할 수 있는 파일의 최대크기|
|encoding|파일의 인코딩방식|
|policy|파일 이름 중복처리를 위한 객체|

## 사용방법
### 먼저 라이브러리를 설치해야한다
  1. <http://www.servlets.com/> 방문
  2. 왼쪽 상단에 com.oreilly.servlet 클릭
  3. 하단에 cos.zip파일 다운로드 후 압축풀기
  4. cos.jar파일 라이브러리에 추가

## 사용예제

1. 보낼려는 파일이 있는 form 태그에서 전송방식이 POST 방식이여야 한다.
2. 그 다음 enctype 속성을 설정해준다. `enctype='multipart/form-data'`

![multi01](./images/multi01.png)

3. 위의 요청을 처리하는 JSP파일에서 MultipartRequest 객체를 생성한다.
  - 파일이 실질적으로 저장될 경로를 구해준다.(디렉토리 경로)
  - 첨부파일의 최대 크기를 설정해준다
  - MultipartRequest객체의 참조변수로 파라미터의 값을 받는다.
## 위의 내용을 코드로 표현
```java
String saveDirectory = request.getRealPath("파일의 웹 경로"); // 실제경로가 리턴된다.
int MaxFileSize = 1024*1024*20; // 20MB로 지정
MultipartRequest mr = new MultipartRequest(request, saveDirectory, maxFileSize, "UTF-8", new DefaultFileRenamePolicy());

// request 객체로 파라미터값을 받을수 없다!! 주의!!
// 참조변수 mr로 파라미터값을 받는다
mr.getParameter("name값");
mr.getParameter("name값2");

```
