# Spring DI(Dependency Injection)

## DI개념 1: 의존성 주입 (의존 객체 주입)
프로젝트 -> 수 많은 클래스 생성 -> 클래스의 서로간의 관계
1. 상속(is ~a)
2. 포함(has ~a)

##### A라는 클래스는 B라는 클래스가 필요하다
##### 1)
```java
class A extends B { }
```
##### 2)
```java
class A {
    B b = new B();
}
```
##### 3)
```java
class A {
    B b;
    A (B b) {
        this.b = b;
    }
}
```

##### 4)
```java
class A {
    public void print(B b) {
        ...
    }
}
```

### 복합연관
필요하는 객체를 [생성자] 통해서 주입(또는 생성) 사용
- 두 객체의 lifecycle은 같다
- 오류: DI는 Spring에서만 존재하는 개념 (x)

```java
public class NewRecordView {
	//점수 출력 (NewRecord 객체가 필요)
	private NewRecord record;
	public NewRecordView(int kor , int eng , int math) {
		record = new NewRecord(kor, eng, math);
	}

	public void print() {
		System.out.println(record.total() + " /" + record.avg());
	}
}
```
### 집합연관
필요한 객체를 [setter 함수]를 통해서 주입 받아서 사용
- 객체의 생성이 서로 독립적이다 > 필요시에 객체를 주입
- 두 객체의 lifecycle은 같지 않다

```java
public class NewRecordView {
	//점수 출력 (NewRecord 객체가 필요)
	private NewRecord record;
	public void setRecord(NewRecord record) { //함수의 parameter로 NewRecord 객체의 주소
		this.record = record;
	}

	public void print() {
		System.out.println(record.total() + " /" + record.avg());
	}
}
```

## 시나리오
Class A, Class B
##### [A라는 클래스가 B라는 클래스를 사용하는 방법]
1. 상속 (is ~a) > Spring Framework에 관심이 없어요
2. 포함 (has ~a)
- [생성자]를 통해서 (constructor injection)
```java
A a = new A();
Class A {
    B b;
    public A() {
        b = new B();
    }
}
```
A라는 클래스의 [생성자]에서 B라는 객체 생성 사용 (복합 연관)
- [setter함수]를 통해서 (setter injection)
```java
A a = new A();
B b = new B();
a.setB(b);
Class A {
    private B b;
    public void setB(B b) {
        this.b = b;
    }
}
```

결론: SpringFrameWork는 다른 객체 참조(의존)하기 위해서 아래 두 방법 사용
* constructor injection
* setter injection
#### -> DI(Dependecny Injection)

## 예제 1
### MessageBean.java
```java
package DI_03_Spring;

public interface MessageBean {
	void sayHello(String name);
}
```

### MessageBean_kr.java
```java
package DI_03_Spring;

public class MessageBean_kr implements MessageBean {
	@Override
	public void sayHello(String name) {
		System.out.println("안녕 " + name + "!");
	}
}
```

### MessageBean_en.java
```java
package DI_03_Spring;

public class MessageBean_en implements MessageBean {
	@Override
	public void sayHello(String name) {
		System.out.println("Hello " + name + "!");
	}
}
```

### DI_03.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans.xsd">
<!--
	IOC 컨테이너 (Spring 전용 메모리 공간) 안에 생성될 객체를 만들고 조립(의존)관계 설정하는 파일
	>> DI_03.xml
	>> main 함수에서 필요한 객체를 만들고 생성하는 것이 아니고 Spring 통해 처리 (제어의 역전)

	영문 MessageBean_en messagebean_en = new MessageBean_en();
	한글 MessageBean_kr messagebean_kr = new MessageBean_kr();
	인터페이스 MessageBean messagebean = new MessageBean_kr();
-->
	<!-- <bean id="message" class="DI_03_Spring.MessageBean_en"></bean> -->
	<bean id="message" class="DI_03_Spring.MessageBean_kr"></bean>
</beans>
```

### HelloApp.java
```java
package DI_03_Spring;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.GenericXmlApplicationContext;

public class HelloApp {
	public static void main(String[] args) {
		//영문
		//MessageBean_en messagebean_en = new MessageBean_en();
		//messagebean_en.sayHello("hong");

		//한글
		//MessageBean_kr messagebean_kr = new MessageBean_kr();
		//messagebean_kr.sayHello("hong");

		//interface 다형성...
		//MessageBean messagebean = new MessageBean_kr();
		//messagebean.sayHello("hong");

		ApplicationContext context = new GenericXmlApplicationContext("classpath:DI_03_Spring/DI_03.xml");
		MessageBean message = context.getBean("message", MessageBean.class);
		message.sayHello("hong");
	}
}
/*
요구사항
MessageBean
영문버전(hong) -> Hello hong!
한글버전(hong) -> 안녕 hong!
결과를 나누어서 출력
인터페이스로 구현 되었으면...

>MessageBean_kr
>MessageBean_en
>위 두개 클래스는 같은 Interface를 구현...
*/
```

## 예제 2
### MessageBean.java
```java
package DI_04_Spring;

public interface MessageBean {
	void sayHello();
}
```

### MessageBeanImpl.java
```java
package DI_04_Spring;

public class MessageBeanImpl implements MessageBean {

	private String name;
	private String greeting;

	//name member field 생성자를 통해서 초기화
	public MessageBeanImpl(String name) {
		this.name = name; //member field 초기화
	}

	//greeting member field는 setter 함수를 통해서 초기화
	public void setGreeting(String greeting) {
		this.greeting = greeting;
	}

	@Override
	public void sayHello() {
		System.out.println(this.name + " / " + this.greeting);
	}
}
```

### DI_04.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans.xsd">
<!--
	IOC 컨테이너 (Spring 전용 메모리 공간) 안에 생성될 객체를 만들고 조립(의존)관계 설정하는 파일
	>> DI_04.xml

	MessageBeanImpl messagebean = new MessageBeanImpl("hong"); //name은 생성자 초기화
	messagebean.setGreeting("hello"); //greeting은 setter로 초기화
	messagebean.sayHello();

	DI문법
	<bean id="한개의 이름 (식별자)"
          name="여러개의 이름" 구분자를 통해서(,), (공백), (;)
	<bean name="m1,m2 m3;m4 class=""
-->
	<bean id="messagebean" name="m1,m2 m3;m4" class="DI_04_Spring.MessageBeanImpl">
		<!--
		<constructor-arg>
			<value>hong</value>
		</constructor-arg>
		-->
		<constructor-arg value="hong" />

		<property name="greeting">
			<value>hello</value>
		</property>
	</bean>
</beans>
```

### HelloApp.java
```java
package DI_04_Spring;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.GenericXmlApplicationContext;

public class HelloApp {
	public static void main(String[] args) {
		//java 코드
		//MessageBeanImpl messagebean = new MessageBeanImpl("hong"); //name은 생성자 초기화
		//messagebean.setGreeting("hello"); //greeting은 setter로 초기화
		//messagebean.sayHello();

		//위 코드를 Spring 통해서 (IOC 컨테이너 안에 객체 만들고 주입을 하고) > xml파일 또는 Annotation 통해서
		ApplicationContext context = new GenericXmlApplicationContext("classpath:DI_04_Spring/DI_04.xml");
		MessageBean messagebean = context.getBean("m2", MessageBean.class);
		messagebean.sayHello();
	}
}
```
