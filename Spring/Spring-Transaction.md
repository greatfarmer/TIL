# Spring Transaction

## 코드
```xml
<beans
  xmlns:tx="http://www.springframework.org/schema/tx"
  xsi:schemaLocation="http://www.springframework.org/schema/tx
http://www.springframework.org/schema/tx/spring-tx.xsd">
```

```xml
<!-- Transaction 만들기 -->
<bean id="transactionManager"
 class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
<property name="dataSource"  ref="driverManagerDataSource" />
</bean>

<tx:annotation-driven  transaction-manager="transactionManager"/>
```

## References
> 참고 1: https://blog.naver.com/tlsdlf5/220692045820

> 참고 2: http://ssmlim.tistory.com/45
