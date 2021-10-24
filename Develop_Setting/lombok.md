# lombok

## 1. Lombok이란 ?

개발을 하면서 VO 클래스를 생성해야 하는 경우가 많은데,VO 클래스의 구조는 기본적으로
멤버변수, 기본생성자, 매개변수있는 생성자, getter, setter 이다.<br>
여기서 멤버변수를 제외한 나머지 생성자와 gettet, setter 메서드는 항상 동일한 형태를 갖는다.

lombok은 어노테이션을 기반으로 코드를 자동완성 해주는 라이브러리이다.<br>
lombok을 이용하면 (매개변수있는)생성자, getter, setter 메서드 등을 자동완성 시켜줄 수 있다.

따라서 코드의 복잡성을 줄이고, 가독성을 향상시킬 수 있다.
<br>
<br>
<br>

## 2. lombok 사용법(6가지 종류만 살펴봄)

```java
@Getter               // getter 메서드 자동으로 추가
@Setter               // setter 메서드 자동으로 추가
@ToString             // 변수들을 기반으로 toString() 메서드 자동으로 오버라이딩 후 추가
@AllArgsConstructor   // 모든 변수를 매개변수로 하는 생성자 자동으로 추가
@NoArgsConstructor    // 기본 생성자 자동으로 추가
@Data                 // @ToString, @EqualsAndHashCode, @Getter, @Setter, @RequiredArgsConstructor를 자동으로 추가

public class DeptVO{
  private int deptno;
  private String dname;
  private String loc;

}

```
<br>
<br>
<br>

## 3. lombok.jar 파일 설치

  1. <https://projectlombok.org/download> 방문
  2. 원하는 경로로 다운로드
  3. cmd창에서 (맥은 터미널) 아래 명령어로 jar파일 실행
  4. > 설치경로> java -jar lombok.jar
  5. Specify location 누른 후 Ecilpse 설치 경로 선택 후 install
  6. 설치 후 Eclipse 실행 경로에 jar 파일이 추가됨
  7. Eclipse 재실행

