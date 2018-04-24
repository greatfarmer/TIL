# DI(Dependency Injection)

## IoC (Inversion of Control - 제어의 역전)
IoC란 간단하게 말하여 프로그램의 제어 흐름 구조가 바뀌는 것이다.

즉, 모든 종류의 작업을 사용하는 쪽에서 제어하는 구조이다.

#### IoC 적용의 장점
Container 기능 -> 객체 간의 결합도를 낮춤

#### IoC 구현 방법
* Constructor Injection
* Setter Injection

#### 요약
기존 Test test = new Test(); 와 같이 객체 생성하면 객체간 결합도가 높음

-> **컨테이너가 객체생성 및 의존성 주입을 처리하도록 '제어의 역전' 구현**

##### *Container
- Tomcat과 같은 WAS(Web Application Server)에 존재
- 객체의 생성과 소멸을 관리 (lifecycle을 관리)
- 예) Servlet은 new를 통해 객체 생성하지 않아도 [Container]내에서 생성되고 관리된다.

> 참고: http://cafe.naver.com/bitcamp104/1320

# XML파일을 통한 스프링 컨테이너의 의존관계 설정

## beans
>출처: http://noritersand.tistory.com/153

```
스프링 컨테이너가 사용할 환경설정 정보를 담고 있는 XML파일의 루트 태그

태그의 속성으로 스프링이 사용할 라이브러리를 정의
```

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans.xsd">
```

## bean
```
객체 생성 태그
```

기존 JAVA 코드
```java
// HelloApp.java
package DI_Spring;

public class HelloApp {
	public static void main(String[] args) {
		MessageBean_kr messagebean_kr = new MessageBean_kr();
	}
}
```

Spring 코드
```xml
<!-- DI.xml -->
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="message" class="DI_Spring.MessageBean_kr"></bean>
</beans>
```
```java
// HelloApp.java
package DI_Spring;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.GenericXmlApplicationContext;

public class HelloApp {
	public static void main(String[] args) {
		ApplicationContext context = new GenericXmlApplicationContext("classpath:DI_Spring/DI.xml");
		MessageBean message = context.getBean("message", MessageBean.class);
	}
}
```

## constructor-arg
```
생성자 주입
```

기존 JAVA 코드
```java
package DI_Spring;
...
MessageBeanImpl messagebean = new MessageBeanImpl("hong");
...
```

Spring 코드
```xml
...
<bean id="messagebean" class="DI_Spring.MessageBeanImpl">
    <constructor-arg>
        <value>hong</value>
    </constructor-arg>
</bean>
...
```
```xml
<!-- 축약 가능 -->
...
<bean id="messagebean" class="DI_Spring.MessageBeanImpl">
    <constructor-arg value="hong" />
</bean>
...
```

## constructor-arg > type
```
생성자 주입 (overloading)
```

```java
package DI_Spring;
...
public class JobExecute {
	public JobExecute(String first, int second) {
		System.out.println("String, int");
	}
	public JobExecute(String first, long second) {
		System.out.println("String, long");
	}
	public JobExecute(String first, String second) {
		System.out.println("String, String");
	}
...
}

public class HelloApp {
    JobExecute jobexecute = new JobExecute("hong", 100); //parameter 처리
    ArticleDao articledao = new ArticleDao();
    jobexecute.setArticledao(articledao);
    jobexecute.setData(500);
...
}

/*
1. <constructor-arg> 순서를 인정
2. <value> 값에 type이 명시되지 않으면 Default >> String
*/
...
```

```xml
...
<bean id="jobexecute" class="DI_Spring.JobExecute">
    <constructor-arg>
        <value>hong</value>
    </constructor-arg>
    <constructor-arg>
        <value type="long">100</value>
    </constructor-arg>
    <property name="articledao">
        <ref bean="articleDAO"/>
    </property>
    <property name="data">
        <value>500</value>
    </property>
</bean>
<bean id="articleDAO" class="DI_Spring.ArticleDao"></bean>
...
```

## constructor-arg > factory-method
```
Singleton
```

```java
package DI_Spring;
...
Singleton single = Singleton.getInstance();
...
```

```xml
...
<bean id="single" class="DI_Spring.Singleton" factory-method="getInstance"></bean>
...
```

## constructor-arg > ref bean
```
참조변수(주소값) 파라미터
```

```java
package DI_Spring;
...
OracleArticleDao articledao = new OracleArticleDao();
WriteArticleService service = new WriteArticleService(articledao);
...
```

```xml
...
<bean id="articledao" class="DI_Spring.OracleArticleDao"></bean>
<bean id="service" class="DI_Spring.WriteArticleService">
    <constructor-arg>
        <ref bean="articledao"/>
    </constructor-arg>
</bean>
...
```

## property
```
setter함수 주입
```

```java
package DI_Spring;
...
messagebean.setGreeting("hello");
...
```

```xml
...
<bean id="messagebean" class="DI_Spring.MessageBeanImpl">
    <property name="greeting"> <!-- setter 함수를 구현하고 있는 변수명 (member field) -->
		<value>hello</value>
	</property>
</bean>
...
```

## list
```
Collection > List
```

```java
package DI_Spring;
...
ProtocolHandler handler = new ProtocolHandler();

ArrayList<MyFilter> arraylist = new ArrayList<>();
arraylist.add(new EncFilter());
arraylist.add(new HeaderFilter());
arraylist.add(new ZipFilter());

handler.setFilters(arraylist);
...
```

```xml
...
<bean id="handler" class="DI_Spring.ProtocolHandler">
    <property name="filters">
        <list> <!-- 재사용하지 않을 것이기에 id 속성은 필요없다 -->
            <bean class="DI_Spring.EncFilter" />
            <bean class="DI_Spring.HeaderFilter" />
            <bean class="DI_Spring.ZipFilter" />
        </list>
    </property>
</bean>
...
```

## map
```
Collection > Map
```

```java
package DI_Spring;
...
ProtocolHandlerFactory factory = new ProtocolHandlerFactory();

Map<String, ProtocolHandler> map = new HashMap<>();
map.put("rss", new RssHandler());
map.put("rest", new RestHandler());

factory.setHandlers(map);
...
```

```xml
...
<bean id="factory" class="DI_Spring.ProtocolHandlerFactory">
    <property name="handlers">
        <map>
            <entry>
                <key><value>rss</value></key>
                <ref bean="rsshandler"/>
            </entry>
            <entry>
                <key><value>rest</value></key>
                <!-- <ref bean="resthandler"/> -->
                <bean class="DI_Spring.RestHandler" />
            </entry>
        </map>
    </property>
</bean>
<bean id="rsshandler" class="DI_Spring.RssHandler"></bean>
...
```

## prop
```
Collection > Properties
```

```java
package DI_Spring;
...
BookClient bookclient = new BookClient();

Properties prop = new Properties();
prop.setProperty("server", "192.168.0.41");
prop.setProperty("connectiontimeout", "20000");

bookclient.setConfig(prop);
...
```

```xml
...
<bean id="bookclient" class="DI_Spring.BookClient">
    <property name="config">
        <!-- Properties 타입을 가진 객체의 주소 주입 -->
        <props>
            <prop key="server">192.168.0.41</prop>
            <prop key="connectiontimeout">20000</prop>
        </props>
    </property>
</bean>
...
```
