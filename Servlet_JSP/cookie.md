# Cookie란?

클라이언트가 서버에 요청을 보낼때, 서버가 클라이언트에게 응답을 할때<br>
모두 쿠키를 주고받으면서 진행할 수 있다.

따라서 쿠키는 서버와 클라이언트가 필요한 정보를 공유하고 유지할 수 있게 해준다!<br>
ex) 페이지 방문횟수, 포탈 검색내역 등

쿠키를 생성, 삭제, 속성을 부여할 수 있는 방법을 알아보겠다.

## Java 객체로 쿠키 생성하기

```java
Cookie c = new Cookie("이름", "내용");
c.setMaxAge(60*60*24*365); // 1년동안 유지
response.addCookie(c);
```

- Cookie 생성자에 쿠키이름과 콘텐츠를 적어준다.
- setMaxAge()메서드로 쿠키 유지기한을 설정해줄 수 있다. (초단위)
- 쿠키를 생성하고 속성값을 부여해주었다면 response객체를 이용해 쿠키를 추가해준다. (삭제 할때도 마찬가지이다.)

```java
Cookie[] cList = request.getCookies();
```

- 쿠키 목록들을 담고 있는 객체배열도 존재한다.

## 쿠키 삭제하기

삭제는 간단하다. 유지기한을 0초로 만들어버리면 그만이다.
```java
c.setMaxAge(0);
response.addCookie(c);
```

## Javascript로 쿠키 생성하기
```javascript
<script>
	window.onload = function(){
		document.cookie = "쿠키명=컨텐츠값";
		document.cookie = "쿠키명=컨텐츠값;expires=Sat, 23 Oct 2021 23:00:00 UTC";
	}
</script>
```

expires로 유지기한을 설정해줄 수 있다.

## jQuery로 쿠키 생성하기

먼저 다운로드 받아야 할 파일이 있다.<br>
<https://plugins.jquery.com/cookie/>

1. jQuery문법으로 쿠키를 생성하기 위해서 jQuery.cookie.js 파일을 다운로드 한다.
2. 해당 프로젝트 webapp 하위폴더에 파일을 복사한다.
```javascript
<script>
	$(function(){
    	// 쿠키생성 : 세션이 종료될때까지 존재
        $.cookie("id", "ahs");
        
        // 5일 뒤에 만료되는 쿠키 생성
        $.cookie("name", "cookie", {expires:5}
        
        // name 쿠키 삭제
        $.removeCookie("name");
    })
</script>
```
