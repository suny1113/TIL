# DI(의존성 주입)
앞서 배웠던 Ioc(제어의 역전)의 개념은 객체를 개발자가 직접 생성하지 않고 객체의 제어권을 스프링 컨테이너에게 맡긴다는 뜻이였다.

즉 직접적으로 의존성을 만들지 않고, 외부에서 의존성을 가져오는 경우를 말한다.

외부에서 객체에게 의존성을 주입해주는 것을 DI라고 하고 Ioc의 일종이라고 생각하면 된다.

이런 방식으로 프로그래밍을 진행하면 객체의 결합도가 느슨해지고 각 클래스들간의 변경이 자유로워지기 때문에 유지보수가 용이하다.

또한 인터페이스를 사용해 클래스들을 구현하면 클래스들 간의 의존관계가 제한이되기 때문에 변경에 자유로워진다.

# 의존성 주입 방법
스프링에서 의존성을 주입하는 방법은 
- 생성자에서 주입
- setter에서 주입
- 필드에서 주입

크게 3가지 방법이 있는데 XML파일에서 Bean을 이용해 생성자와 setter에서 주입하는 경우를 정리해보았다.

### 1. 먼저 인터페이스를 생성한 후 인터페이스를 구현한 클래스를 작성한다.
```java
package kr.co.spring.test1;

// Weapon 인터페이스
public interface Weapon {
	void fire();
	void reload();
}

```

```java
package kr.co.spring.test1;

// Weapon 인터페이스를 구현한 MachineGun 클래스
public class MachineGun implements Weapon {
	
	int bullet;
	UpgradeWeapon uw;
	
	// 생성자
	public MachineGun(int bullet, UpgradeWeapon uw) {
		super();
		this.bullet = bullet;
		this.uw = uw;
	}
	

	@Override
	public void fire() {
		System.out.println("두두");
		bullet --;
		
	}


	@Override
	public void reload() {
		System.out.println("reload..");
		bullet = 100;
		
	}

}
```

```java
package kr.co.spring.test1;

// UpgradeWeapon 클래스
public class UpgradeWeapon {
	
	public void upgrade() {
		System.out.println("power10 증가");
	}
	
}
```

### 2. UpgradeWeapon 클래스의 메서드를 MachineGun 클래스에서 사용하기 위해 객체를 직접 생성하지 않고 의존성을 주입받는 방식으로 작성한다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

  <!-- UpgradeWeapon uw = new UpgradeWeapon()과 동일-->
	<bean id="uw" class="kr.co.spring.test1.UpgradeWeapon"></bean>

  <!-- constructorDI -->
	<bean id="mg" class="kr.co.spring.test1.MachineGun">
		<constructor-arg name="bullet" value="100" />
		<constructor-arg name="uw" ref="uw" />
	</bean>
</beans>

```

### 2-1 setter를 통해 의존성을 주입하려면 다음과 같이 작성
```java
// setter
	public void setBullet(int bullet) {
		this.bullet = bullet;
	}
	
	// setter
	public void setUw(UpgradeWeapon uw) {
		this.uw = uw;
	}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

	<bean id="uw" class="kr.co.spring.test1.UpgradeWeapon"></bean>
	
  <!-- setterDI -->
	<bean id="mg" class="kr.co.spring.test1.MachineGun">
		<property name="bullet" value="100"/>
		<property name="uw" ref="uw" />
	</bean>
</beans>

```

### 3. 의존성을 주입받은 객체 사용
```java
package kr.co.spring.test1;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class TestMain {

	public static void main(String[] args) {
		ApplicationContext ac = new ClassPathXmlApplicationContext("application.xml");
		
		Weapon weapon = ac.getBean("mg", MachineGun.class);
		
		weapon.fire();
		weapon.reload();

	}

}
```

