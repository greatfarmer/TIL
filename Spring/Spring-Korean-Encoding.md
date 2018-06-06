# Spring 한글 처리(UTF-8)

## Spring MVC 프로젝트 설정
### web.xml
```xml
<filter>
  <filter-name>EncodingFilter</filter-name>
  <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
  <init-param>
    <param-name>encoding</param-name>
    <param-value>UTF-8</param-value>
  </init-param>
</filter>
<filter-mapping>
  <filter-name>EncodingFilter</filter-name>
  <url-pattern>/*</url-pattern>
</filter-mapping>
```

## Tomcat 설정
### server.xml
```xml
<!-- server.xml -->
<Connector URIEncoding="UTF-8" connectionTimeout="20000" port="8090"protocol="HTTP/1.1"redirectPort="8443"/>
```
