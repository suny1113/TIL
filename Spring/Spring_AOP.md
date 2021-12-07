# Spring AOP (Aspect Oriented Programming : 관점 지향 프로그래밍)

여러 객체에 공통으로 적용될 수 있는 기능을 분리함으로써 재사용성을 높여주는 프로그래밍 기법

1. 업무 로직을 포함하는 기능 : 핵심(target)기능(Core Concern)
2. 핵심기능을 도와주는 기능 : 부가(advice)기능(Cross-cutting Concerns)

두가지 기능으로 분리할 수 있다.

## Aspect란? (advice + pointercut)
- 부가기능을 정의한 advice와 advice를 어디에 적용할것인가(join point)를 결정하는 pointercut을 합친 개념이다.
- weaving : pointercut에 의해 결정된 target의 join point에 부가기능을 삽입하는 과정
- spring은 런타임시 프록시 객체를 생성해서 공통기능을 추가하는 방법으로 AOP를 적용한다.
