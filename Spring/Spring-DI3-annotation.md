# Annotation
> 참고 [전자정부 표준프레임워크 위키](http://www.egovframe.go.kr/wiki/doku.php?id=egovframework:rte2:ptl:annotation-based_controller), [놀이터흙이제맛](http://noritersand.tistory.com/156?category=4061)

## @Autowired
```
by type 으로 주입
```
### 목적
```
의존관계를 자동설정할 때 사용하며 타입을 이용하여 의존하는 객체를 삽입해 준다.
그러므로 해당 타입의 빈객체가 존재하지 않거나 또는 2개 이상 존재할 경우 스프링은 예외를 발생시키게 된다.
```
### 설정 위치
```
생성자, 필드, 메소드(굳이 setter메소드가 아니여도 된다)
```
### 추가설정
```
AutowiredAnnotationBeanPostProcessor 클래스를 빈으로 등록시켜줘야 한다.
해당 설정 대신에 <context:annotation-config> 태그를 사용해도 된다.
```
### 옵션
```
required - @Autowired어노테이션을 적용한 프로퍼티에 대해 굳이 설정할 필요가 없는 경우에
false값을 주며 이때 해당 프로퍼티가 존재하지 않더라도 스프링은 예외를 발생시키지 않는다.
디폴트값은 true
```
### 1. XML 사용 코드
```java
// MonitorViewer.java
package DI_Annotation_01;

import org.springframework.beans.factory.annotation.Autowired;

public class MonitorViewer {
	private Recorder recorder;
	public Recorder getRecorder() {
		return recorder;
	}

    // Annotation 기반으로 DI 작업 (setter 주입을 사용)
	@Autowired
	public void setRecorder(Recorder recorder) {
		this.recorder = recorder;
	}
}
```
```xml
<!-- DI_Annotation_01.xml -->
...
<context:annotation-config />
<bean id="viewer" class="DI_Annotation_01.MonitorViewer"></bean>
<bean id="recorder" class="DI_Annotation_01.Recorder"></bean>
...
```

### 2. JAVA 사용 코드 (함수 기반의 처리)
```
@configuration (설정파일)
@Bean (객체 생성)
```
```java
// Configcontext.java
package DI_Annotation_05;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

/*
Configcontext를 Spring 설정 파일로 사용하겠다 (xml 파일 대체 하겠다)
: 객체 생성과 주입을 처리하겠다

xml파일이라면
<bean id="user" class="DI_Annotation_05.User">
Java 파일에서는 함수를 생성해서 객체 주소 리턴하는 형태
*/

@Configuration //xml 생성
public class Configcontext {
	@Bean
	public User user() { //<bean id="user" class="DI_Annotation_05.User">
		return new User();
	}
}
```
```java
// Program.java
package DI_Annotation_05;

import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class Program {
	public static void main(String[] args) {
        // xml코드와 동일하게 사용
		ApplicationContext context = new AnnotationConfigApplicationContext(Configcontext.class);

		User user = context.getBean("user", User.class);
		user.userMethod();
	}
}
```

### @Autowired 의 옵션: required 강제
```
default는 true 무조건 주입
required = false는 객체가 존재하면 주입, 없으면 null값 반환
```
```java
// MonitorViewer.java
package DI_Annotation_03;

...
@Autowired(required = false) //Default > true (무조건 injection)
public void setRecorder(Recorder recorder) {
    this.recorder = recorder;
    System.out.println("setter 주입 성공");
}
...
```
```xml
<!-- DI_Annotation_03.xml -->
...
    <context:annotation-config />
	<bean id="monitorViewer" class="DI_Annotation_03.MonitorViewer"></bean>
    <!-- 객체 생성하지 않았을 경우 -->
	<!-- <bean id="xx" class="DI_Annotation_03.Recorder"></bean> -->
...
```
console 결과
```
null
```
### @Scope("prototype") 사용
```
scope="prototype"
getbean() 호출시 새로운 객체 리턴 (새로운 객체... new...)
```
```java
// JavaConfigPrototype.java
package Config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Scope;

import Spring.Client;

//xml > java 파일 > prototype 설정

@Configuration
public class JavaConfigPrototype {

/* xml파일의 경우
    <bean id="client" class="Spring.Client" scope="prototype">
        <property name="host" value="webserver"></property>
    </bean>
*/
	@Bean
	@Scope("prototype")
	public Client client(){
		Client client = new Client();
		client.setHost("webserver");
		return client;
	}
}
```
```xml
<!-- ApplicationContext.xml -->
...
<bean id="client" class="Spring.Client" scope="prototype">
    <property name="host" value="webserver"></property>
</bean>
...
```
```java
// MainPrototypeConfig.java
package main;

import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

import Spring.Client;
import Config.JavaConfigPrototype;

public class MainPrototypeConfig {
	public static void main(String[] args) {
		ApplicationContext context =
				new AnnotationConfigApplicationContext(JavaConfigPrototype.class);
		Client client =  context.getBean("client", Client.class);
		Client client2 = context.getBean("client", Client.class);
		System.out.println("client : " + client.toString());
		System.out.println("client2 : " + client2.toString());
	}
}
```
console 결과
```
client : Spring.Client@1ad282e0
client2 : Spring.Client@7f416310
```
## @Qualifier
### 목적
```
@Autowired의 목적에서 동일 타입의 빈객체가 존재시 특정빈을 삽입할 수 있게 설정한다.
```
### 설정위치
```
@Autowired 어노테이션과 함께 사용된다.
```
### 추가설정
```
동일타입의 빈객체 설정에서 <qualifier value="[alias명]" />를 추가하여 준다.
```
### 옵션
```
name - alias명
```
### 코드
```java
// MonitorViewer.java
package DI_Annotation_02;
...
@Autowired
@Qualifier("corder1") //<qualifier value="corder1" />
public void setRecorder(Recorder recorder) {
    this.recorder = recorder;
    System.out.println("recorder: " + recorder);
}

@Autowired
@Qualifier("corder2") //<qualifier value="corder2"></qualifier>
public void RecorderMethod(Recorder rec) {
    System.out.println("rec: " + rec);
}
...
```
```xml
<!-- DI_Annotation_02.xml -->
...
    <context:annotation-config />
	<bean id="monitorViewer" class="DI_Annotation_02.MonitorViewer"></bean>
	<bean id="xx" class="DI_Annotation_02.Recorder">
		<qualifier value="corder1" />
	</bean>
	<bean id="yy" class="DI_Annotation_02.Recorder">
		<qualifier value="corder2"></qualifier>
	</bean>
...
```
console 결과
```
rec: DI_Annotation_02.Recorder@3e57cd70
recorder: DI_Annotation_02.Recorder@9a7504c
```

## @Resource
```
by name (by id) 으로 주입

@Resource 쓰는 경우는 여러 객체가 만들어질때 다른 생성자를 사용하는 경우
```
### 목적
```
어플리케이션에서 필요로 하는 자원을 자동 연결(의존하는 빈 객체 전달)할 때 사용
@Autowired 와 같은 기능을 하며 @Autowired와 차이점은 @Autowired는 타입으로(by type),
@Resource는 이름으로(by name)으로 연결시켜준다는 것이다.
```
### 설정위치
```
프로퍼티, setter메소드
```
### 추가설정
```
CommonAnnotationBeanPostProcessor 클래스를 빈으로 등록시켜줘야 한다.
해당 설정 대신에 <context:annotation-config> 태그를 사용해도 된다.
```
### 옵션
```
name
```
### 코드
```java
// MonitorViewer.java
package DI_Annotation_04;

import javax.annotation.Resource;
...
@Resource(name="xx") //id값 또는 name값을 명시해서 특정 객체 주입
public void setRecorder(Recorder recorder) {
    this.recorder = recorder;
    System.out.println("setter 주입 성공");
}
...
```
```xml
<!-- DI_Annotation_04.xml -->
...
    <context:annotation-config />
	<bean id="monitorViewer" class="DI_Annotation_04.MonitorViewer"></bean>
	<bean id="xx" class="DI_Annotation_04.Recorder"></bean>
	<bean id="yy" class="DI_Annotation_04.Recorder"></bean>
	<bean id="zz" class="DI_Annotation_04.Recorder"></bean>
...
```
