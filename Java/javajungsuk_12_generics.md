> '자바의정석 기초편 -남궁성' 12장 학습
>> 1. 지네릭스란?



# 1. 지네릭스(Generics)
- 다양한 타입의 객체들을 다루는 메서드나 컬렉션 클래스들의 컴파일 시 타입 체크를 해주는 기능
- 객체의 타입 안정성을 높이고 형변환의 번거로움이 줄어든다.
- 형변환 생략 -> 코드의 간결화

### 예시
```java
ArrayList<Integer> list = new ArrayList<Integer>();
```
- Integer 타입의 객체만 받을 수 있는 ArrayList 클래스
- 지네릭 타입은 참조변수 앞, 생성자 앞에 지정해준다.

### 지네릭스의 용어
```java
class Box<T>{}
```
1. Box<T>를 지네릭클래스 라고 한다.
2. T는 타입변수 또는 타입매개변수라 한다.
3. Box는 원시타입이라고 한다. (컴파일 시 지네릭 타입이 제거되고 원시타입만 남는다.)

### 지네릭의 다형성
  
```java
class Product{}
class Tv extends Product{}
class Radio extends Product{}
  
ArrayList<Product> list = new ArrayList<Product>();
List<Product> list2 = new ArrayList<Product>();

list.add(new Product());
list.add(new Tv());
list.add(new Radio());
  
Tv t = (Tv)list.get(i);
Radio r = (Radio)list.get(i);
```
- 참조변수의 지네릭타입과 생성자의 지네릭 타입은 일치해야 한다.
- 클래스 타입 간에 다형성을 적용할 때에도 지네릭 타입은 일치해야 한다.
- 지네릭 타입의 자손 객체는 저장할 수 있다. -> 저장된 객체를 꺼낼때는 형변환 필요
  
### 지네릭 타입의 제한
- 지네릭 타입에 'extends'를 사용하면, 특정 타입의 자손들만 대입할 수 있게 제한한다.
```java
class ProductBox<T extends Product>{}
```
- Product클래스의 자손들만 대입할 수 있다.
```java
class ProductBox<T extends Producible>{}
```
- 특정 인터페이스를 구현한 클래스만 대입해야 할때도 extends키워드를 사용한다.

```java
class ProductBox<T extends Product & producible>{}
```
- 특정 클래스의 자손이면서 특정 인터페이스를 구현해야 한다면 & 기호로 연결해준다.

  
