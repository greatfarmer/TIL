# Spring MVC패턴

![spirngMVC](images/SpringMVC_summary.jpg)

## web.xml
```xml
<!-- /WEB-INF/web.xml -->
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi=...>
  <display-name>SpringMVC_Basic01_Controller</display-name>
  <welcome-file-list>
    <welcome-file>index.html</welcome-file>
  </welcome-file-list>
  <servlet>
  	<servlet-name>spring</servlet-name>
    <!-- DispatcherServlet 클래스 (Spring 제공하는 FrontController, 요청 판단(url-pattern 설정)) -->
  	<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
  </servlet>
  <servlet-mapping>
  <!--
    **약속   <servlet-name>spring</servlet-name>
              : spring-servlet.xml 자동  read >> DispatcherServlet
        예)  <servlet-name>a</servlet-name>
              : 자동 : a-servlet.xml
  -->
  	<servlet-name>spring</servlet-name>
  	<url-pattern>*.do</url-pattern>
  </servlet-mapping>
</web-app>
```

## spring-servlet.xml
```xml
<!-- /WEB-INF/spring-servlet.xml -->
<!--
ViewResolver: view 설정을 담당하는 클래스
import org.springframework.web.servlet.view.InternalResourceViewResolver;
-->
...
<!-- id="/intro.do" >> url-pattern >> mapping 주소 -->
<bean id="/intro.do" class="kr.or.bit.IntroController"></bean>

<bean id="internalResourceViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    <property name="prefix">
        <value>/WEB-INF/views/</value>
    </property>
    <property name="suffix">
        <value>.jsp</value>
    </property>
</bean>
<!--
    ModelAndView mav = new ModelAndView();
    mav.addObject("name", "kang"); //request.setAttribute("name", "kang");
    mav.setViewName("intro");

    경로
    /WEB-INF/views/ + intro + .jsp

    view주소: /WEB-INF/views/intro.jsp
-->
...
```

## IntroController.java
```java
// /src/kr/or/bit/IntroController.java
package kr.or.bit;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.mvc.Controller;

public class IntroController implements Controller{
	public IntroController() {
		System.out.println("IntroController 객체 생성");
	}

	//handleRequest > servlet (doGET, doPOST) 역할
	@Override
	public ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception {
		System.out.println("IntroController 요청 실행: handleRequest");
		ModelAndView mav = new ModelAndView();
		mav.addObject("name", "kang"); //request.setAttribute("name", "kang");
		mav.setViewName("intro"); // WEB-INF/views/intro.jsp
		return mav;
	}
}
```

## intro.jsp
```html
<!-- /WEB-INF/views/intro.jsp -->
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Intro</title>
</head>
<body>
	<h3>Intro</h3>
	VIEW: ${name} <!--EL사용 request.getAttribute("name"); -->
    <!-- 결과 >> VIEW: kang -->
</body>
</html>
```

## index.html
```html
<!-- /index.html -->
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<h3>Spring MVC</h3>
<!-- http://localhost:8090/SpringMVC_Basic01_Controller/intro.do -->
<a href="intro.do">intro.do 요청</a>
</body>
</html>
```

# Annotation 활용
## web.xml
```xml
<!-- /WEB-INF/web.xml -->
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd" id="WebApp_ID" version="3.1">
  <display-name>SpringMVC_Basic03_Annotation</display-name>
  <welcome-file-list>
    <welcome-file>index.html</welcome-file>
  </welcome-file-list>
  <servlet>
  	<servlet-name>dispatcher</servlet-name>
  	<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
  </servlet>
  <servlet-mapping>
  	<servlet-name>dispatcher</servlet-name>
  	<url-pattern>*.do</url-pattern>
  </servlet-mapping>
</web-app>
```

## dispatcher-servlet.xml
```xml
<!-- /WEB-INF/dispatcher-servlet.xml -->
...
<!--
자동 주입
의존관계(생성자, setter) 자동 주입
xml에서 안하고 >> @Autowired, @Resource 처리
-->
	<context:annotation-config />
<!-- 공통 빈 -->
	<bean id="internalResourceViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<property name="prefix">
			<value>/WEB-INF/views/</value>
		</property>
		<property name="suffix">
			<value>.jsp</value>
		</property>
	</bean>
<!-- TEST_1
@Controller
public class HelloController

@Controller 가지고 있는 클래스는 자동으로 IOC 컨테이너에 등록되는 방법은?

@Component, @Repository,
@Service, @Controller,
@RestController, @ControllerAdvice,
@Configuration
있는 클래스를 자동으로 bean객체 생성 가능
<context:component-scan base-package="com.controller" />
-->
	<bean id="helloController" class="com.controller.HelloController"></bean>

<!-- TEST_2 설정 (하나의 요청 주소: GET, POST 처리)
	화면단, 처리단 (로그인 화면, 로그인 처리: 글쓰기 화면, 글쓰기 처리)
	전제조건: 요청되는 주소가 같다
	@Autowired > ArticleService
-->
	<bean class="com.controller.NewArticleController"></bean>
	<bean class="com.service.ArticleService"></bean>
...
```

## NewArticleController.java
```java
// /src/com/controller/NewArticleController.java
package com.controller;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

import com.model.NewArticleCommand;
import com.service.ArticleService;

@Controller
@RequestMapping("/article/newArticle.do") //GET방식 요청, POST방식 요청
public class NewArticleController {
	private ArticleService articleservice;

	@Autowired
	public void setArticleservice(ArticleService articleservice) {
		this.articleservice = articleservice;
	}

	//**하나의 요청 주소로 2가지 판단 (GET, POST) > method=GET, method=POST
	//글쓰기(GET), 글쓰기 완료(POST)

	//GET 방식 요청 (사용자 화면단 제공)
	@RequestMapping(method=RequestMethod.GET)
	public String form() {
		System.out.println("GET 방식에 대한 요청");
		return "article/newArticleForm";

		//ViewResolver에 의해서
		// /WEB-INF/views/article/newArticleForm.jsp
	}

	//1. 전통적으로 Client 요청 데이터 받기 (Spring에서 더 이상 사용 안함)
	/*
	@RequestMapping(method=RequestMethod.POST)
	public String submit(HttpServletRequest request) {
		NewArticleCommand article = new NewArticleCommand();
		article.setParentId(Integer.parseInt(request.getParameter("parentId")));
		article.setTitle(request.getParameter("title"));
		article.setContent(request.getParameter("content"));
		articleservice.writeArticle(article);

		return "article/newArticleSubmitted";
	}
	*/

	//public String submit(NewArticleCommand command) {
	//동작원리: JSP (UserBean Action 태그 사용: setProperty ...), jQuery Serialize와 유사

	//전제조건: Form 태그의 name 속성의 이름이 DTO 객체의 memberfield와 같아야 한다.
	//<input type="text" name="title"> >> private String title;

	//submit(NewArticleCommand command) 함수의 parameter DTO 타입을 사용
	//넘어오는 parameter가 DTO타입의 memberfield명과 같다면
	//1. 자동으로 DTO 객체 생성: NewArticleCommand newArticleCommand = new NewArticleCommand();
	//2. 자동으로 넘어온 parameter 값을 setter함수를 이용하여 자동 주입
	//1.1 NewArticleCommand 객체 IOC 컨테이너에 id="newArticleCommand" 자동 생성...
	//원칙: ModelAndView mv = new ModelAndView(); mv.addObject("newArticleCommand", newArticleCommand); return mv;
	//위 원칙이 없어도 view 페이지에 DTO객체 (NewArticleCommand) 주소가 자동 forward

	/*
		1. submit(NewArticleCommand command)
			>자동 객체 생성되고 객체변수명이 (key): newArticleCommand

		2. 이름이 자동으로 생성되는 것이 싫다면
			submit(@ModelAttribute("Articledata")NewArticleCommand command)
			>>자동으로 생성되는 객체변수명을 제어 (Articledata 강제) key: Articledata

		3. Model.addAttribute("Articledata", new NewArticleCommand()); 자동으로

	*/

	//POST 방식 요청 (DB단 처리 Insert 처리)
	@RequestMapping(method=RequestMethod.POST)
	// public String submit(NewArticleCommand command) {
	public String submit(@ModelAttribute("Articledata")NewArticleCommand command) {
		//1. parameter 받기
		//2. Service 객체 생성하기 ArticleService service = new ArticleService()
		//3. Service 객체 함수 호출하기
		//4. 결과 받아서 return 하기

		articleservice.writeArticle(command);
		return "article/newArticleSubmitted";
	}
}
```
