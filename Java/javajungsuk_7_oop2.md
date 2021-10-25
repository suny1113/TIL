# 객체지향 프로그래밍2
>'자바의정석 기초편-남궁성'7장학습
>> 1. 상속
>> 2. 포함
>> 3. 단일상속
>> 4. 오버라이딩
>> 5. 참조변수super, 생성자super()
>> 6. 접근 제어자
>> 7. 다형성
>> 8. 추상클래스
>> 9. 인터페이스


## 1. 상속 (A is B)
- 기존의 클래스를 재사용하여 새로운 클래스를 작성
  - 코드의 재사용성을 높이고 중복을 제거한다.
  - 자손 클래스는 부모의 모든 멤버를 상솓받는다.
  - 멤버의 갯수는 항상 자손 >= 조상
  - extends 키워드를 사용
```java
public class Child extends Parent{}
```
## 2. 포함(A has a B)
- 클래스의 멤버변수로 다른 클래스 타입의 참조변수를 선언
```java
public class Engine{}
```
```java
public class Car{
  Engine e = new Engine();
}
```
- Car 클래스가 Engine타입을 포함한다.

## 3. 단일상속
- 자바에서는 단일 상속만을 허용한다.
- 클래스 간의 관계가 명확해지고 코드의 신뢰도가 높아진다.

## 4. 오버라이딩
- 조상 클래스의 메서드를 상속받아 자손클래스의 기능에 맞추어 적절하게 변경하여 사용하는 것
  - 조건1 : 메서드의 선언부가 완전히 일치
  - 조건2 : 접근 제어자의 범위가 조상의 것과 같거나 더 넓어야 한다.
  - 조건3 : 조상의 것보다 많은 예외를 선언할 수 없다.

## 5. 참조변수 super, 생성자 super();
- 참조변수 super는 조상의 멤버를 가리키며 조상과 자손의 멤버의 이름이 같을때 구분짓기 위해 사용한다.
```java
class Parent{int i = 10}; // super.i
class Child extends Parent{int i = 20}; // this.i

```

- 생성자 super()는 조상의 생성자를 호출할 때 사용한다.
- 조상의 멤버는 조상의 생성자로, 자손의 멤버는 자손의 생성자로 초기화시키는 것이 바람직하다.
```java
class Tv{
 String name;
 int button;
 
 Tv(String name, int button){
    this.name = name;
    this.button = button;
 }
}

class SmartTv extends Tv{
  String text;
  
  SmartTv(String text, String name, int button){
    super(name, button);
    this.text = text;
  }
}
```
## 6. 접근 제어자
- 멤버 또는 클래스에 사용되어, 외부에서 접근하지 못하게 제한하는 역할을 한다.
- 접근 제어자의 종류 (범위가 넓은 순서부터)
  - public : 접근 제한이 없다.
  - protected : 다른 패키지의 자손클래스에서까지 접근이 가능하다.
  - (default) : 생략되어 있는 형태이며 같은 패키지 내에서만 접근이 가능하다.
  - private : 같은 클래스 내에서만 접근이 가능하다.

### 접근 제어자를 선언하는 이유(캡슐화)
- 클래스 내부의 데이터를 보호하기 위함이다.
- 보호할 멤버들을 private 제어자로 선언해 외부에서 값을 변경시키지 못하게 막아야 한다.
- 멤버변수의 값에 접근할려면 get 메서드와 set 메서드를 이용한다.
```java
class Score{
  private int score;
  
  public int getScore(){ return score; }
  public void setScore(){
    if(score < 0 || score >100) return;
    this.score = score;
  }
}
public class Test(){
  public static void main(String args[]){
      Score s = new Score();
    //s.score = 50; 에러, private 변수 직접 접근 불가
      s.setScore(50);
      int score = s.getScore(); // 정상적인 방법
  }
}
```
## 7. 다형성
- 조상타입의 참조변수로 자손타입의 객체를 참조할 수 있다. (타입 불일치)
- 사용할 수 있는 멤버의 개수가 달라진다.
```java
Parent p = new Child();
```
- 상속관계에서 참조변수의 형변환이 가능하다.

## 8. 추상클래스
- abstract 키워드 사용
- 미완성설계도에 비유할 수 있다.
- 추상메서드를 포함하고 있으며 인스턴스를 생성할 수 없다.
- 상속을 통해 미완성 메서드를 구현시켜주어야 한다.
- 멤버, 생성자 모두 가질 수 있다.
```java
abstract class A{
  int a;
  int b;
  
  A(){}
  abstract void B(); // 추상메서드
}
```
### 추상메서드
- 상속하는 클래스에 따라서 메서드의 구현부가 변경될것으로 예상되는 메서드에 추상화를 시킨다.

## 9. 인터페이스
- implements 키워드를 사용해 구현시킨다.
- 추상 클래스보다 추상화의 정도가 높다 -> 멤버변수와 생성자 가질 수 없다.
- static 메서드, 디폴트 메서드, 상수, 추상메서드만 멤버로 가질 수 있다.
```java
interface 인터페이스명{
  public static final 타입 상수명 = 값;
  public abstract 메서드명(매개변수);
}
```
- 상수는 public static final이 생략된 형태로 작성 가능
- 추상메서드는 public abstract가 생략된 형태로 작성 가능
- 인터페이스는 다중상속이 가능하다.
  - 장점
  - 개발시간 단축
  - 표준화 가능
  - 서로 관련없는 클래스들 간의 관계를 맺어줌
  - 독립적 프로그래밍 가능



