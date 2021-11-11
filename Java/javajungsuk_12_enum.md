## enum 열거형
- 여러 상수를 선언해야 할 때 편리하게 선언할 수 있는 방법
- `enum 열거형이름{ 상수명1, 상수명2, ...}`
- 열거형 사용방법 : 열거형이름.상수명(static변수 참조와 동일)
```java
class Car{
	enum Kind{
		HYBRID, GASOLINE, DIESEL, ELECTRIC
	}
	
	enum Type{
		SUV, SPORT, SMALL, SEDAN
	}
	
	public static void main(String[] args) {
		final Kind kind;
		
		final Type type;
	}
}

}
```

