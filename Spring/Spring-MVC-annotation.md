# Spring MVC Annotation

## 공통 코드
### web.xml
```xml
<!-- /WEB-INF/web.xml -->
<web-app ...>
...
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

### dispatcher-servlet.xml
```xml
<!-- /WEB-INF/dispatcher-servlet.xml -->
<beans ...>
<!-- @Autowired DI처리 -->
	<context:annotation-config />
<!-- ViewResolver -->
	<bean id="internalResourceViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<property name="prefix">
			<value>/WEB-INF/views/</value>
		</property>
		<property name="suffix">
			<value>.jsp</value>
		</property>
	</bean>
<!-- TEST_1 -->
	<bean id="helloController" class="com.controller.HelloController"></bean>

<!-- TEST_2 설정 (하나의 요청 주소: GET, POST 처리)
	전제조건: 요청되는 주소가 같다 -->
	<bean class="com.controller.NewArticleController"></bean>
	<bean class="com.service.ArticleService"></bean>

<!-- TESt_3 설정  (하나의 요청 주소 : GET , POST 처리) : List<> -->
    <bean class="com.controller.OrderController"></bean>
</beans>
```

## TEST_1 Annotation 사용
### HelloController.java
```java
// /src/com/controller/HelloController.java
package com.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.ModelAndView;

//@Controller >> 함수 단위로 mapping 작업이 가능  (@RequestMapping)
@Controller
public class HelloController {
	@RequestMapping("/hello.do")
	public ModelAndView hello() {
		ModelAndView mv = new ModelAndView();
		mv.addObject("greeting", "getGreeting()");
		mv.setViewName("Hello");
        // view주소: /WEB-INF/views/Hello.jsp
		return mv;
	}

	//필요하다면 일반함수 만들어서 사용가능
	public String getGreeting() {
		String data = "안녕하세요";
		return data;
	}
}
```

### Hello.jsp
```html
<!-- /WEB-INF/views/Hello.jsp -->
...
<body>
    View: ${greeting}
    <!-- View: 안녕하세요 -->
</body>
...
```

## TEST_2: 하나의 요청 주소: GET, POST 처리

### NewArticleCommand.java (DTO)
```java
// /src/com/model/NewArticleCommand.java
package com.model;

public class NewArticleCommand {
	private int parentId;
	private String title;
	private String content;

	public int getParentId() {
		return parentId;
	}
	public void setParentId(int parentId) {
		this.parentId = parentId;
	}
	public String getTitle() {
		return title;
	}
	public void setTitle(String title) {
		this.title = title;
	}
	public String getContent() {
		return content;
	}
	public void setContent(String content) {
		this.content = content;
	}

	@Override
	public String toString() {
		return "NewArticleCommand [parentId=" + parentId + ", title=" + title + ", content=" + content + "]";
	}
}
```

### ArticleService.java
```java
// /src/com/service/ArticleService.java
package com.service;

import com.model.NewArticleCommand;

public class ArticleService {
	public ArticleService() {
		System.out.println("ArticleService 객체 생성자 호출");
	}

	public void writeArticle(NewArticleCommand command) {
		System.out.println("글쓰기 작업 완료: " + command.toString());
	}
}
```

### NewArticleController.java
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
    //@@RequestMapping(value="/article/newArticle.do", method=RequestMethod.GET)
	@RequestMapping(method=RequestMethod.GET)
	public String form() {
		System.out.println("GET 방식에 대한 요청");
		return "article/newArticleForm";
		// /WEB-INF/views/article/newArticleForm.jsp (ViewResolver에 의해서)
	}

    //POST 방식 요청 (DB단 처리 Insert 처리)

	//전제조건: Form 태그의 name 속성의 이름이 DTO 객체의 memberfield와 같아야 한다.
	//<input type="text" name="title"> >> private String title;

    /* 자동으로 되는 것들
        1. parameter 받기
        2. Service 객체 생성하기 ArticleService service = new ArticleService()
        3. Service 객체 함수 호출하기
        4. 결과 받아서 return 하기
    */

    //@@RequestMapping(value="/article/newArticle.do", method=RequestMethod.POST)
	@RequestMapping(method=RequestMethod.POST)
	// public String submit(NewArticleCommand command) {
	public String submit(@ModelAttribute("Articledata")NewArticleCommand command) {
		articleservice.writeArticle(command);
		return "article/newArticleSubmitted";
	}
}
```

### newArticleForm.jsp
```html
<!-- /WEB-INF/views/article/newArticleForm.jsp -->
...
<body>
	<h3>게시판 글쓰기 입력 폼</h3>
	<h3>form 태그에 action 주소가 없으면 현재 URL 창에 주소가 요청 주소</h3>
	<form method="post">
		<input type="hidden" name="parentId" value="0">
		제목: <input type="text" name="title"><br>
		내용: <input type="text" name="content"><br>
			<input type="submit" value="전송">
	</form>
</body>
...
```

### newArticleSubmitted.jsp
```html
<!-- /WEB-INF/views/article/newArticleSubmitted.jsp -->
...
<body>
	<h3>게시판 입력 내용 확인</h3>
	제목: ${Articledata.title}<br>
	내용: ${Articledata.content}<br>
	순번: ${Articledata.parentId}<br>
</body>
...
```

## TEST_3: 하나의 요청 주소 : GET , POST 처리 (List 컬렉션 처리)

### OrderItem.java (DTO)
```java
// /src/com/model/OrderItem.java
package com.model;

public class OrderItem {
	private int itemid;
	private int number;
	private String remark;

	public int getItemid() {
		return itemid;
	}
	public void setItemid(int itemid) {
		this.itemid = itemid;
	}
	public int getNumber() {
		return number;
	}
	public void setNumber(int number) {
		this.number = number;
	}
	public String getRemark() {
		return remark;
	}
	public void setRemark(String remark) {
		this.remark = remark;
	}

	@Override
	public String toString() {
		return "OrderItem [itemid=" + itemid + ", number=" + number + ", remark=" + remark + "]";
	}
}
```
### OrderCommand.java
```java
// /src/com/model/OrderCommand.java
package com.model;

import java.util.List;

//주문서 클래스
//하나의 주문에 여러개의 상품을 살 수 있다

//OrderCommand > Orderitem, OrderItem , OrderItem, ... 가질수 있다
public class OrderCommand { //하나의 주문서 (여러개의 상품)
	private List<OrderItem> orderItem;

	public List<OrderItem> getOrderItem() {
		return orderItem;
	}

	public void setOrderItem(List<OrderItem> orderItem) {
		this.orderItem = orderItem;
	}

	/*
	 OrderCommand command = new OrderCommand();

	 //key point
	 List<OrderItem> list = new ArrayList<>();
	 list.add(new OrderItem(1,10,"파손주의"));
	 list.add(new OrderItem(2,100,"남성"));
	 list.add(new OrderItem(3,111,"사이즈 교환 불가"));
	 command.setOrderItem(list)

     Map > HashMap<,>		 
	*/
}
```

### OrderController.java
```java
// /src/com/controller/OrderController.java
package com.controller;

import java.util.List;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

import com.model.OrderCommand;
import com.model.OrderItem;

@Controller
@RequestMapping("/order/order.do")
public class OrderController {
	@RequestMapping(method=RequestMethod.GET) //화면
	public String form() {
		return "order/OrderForm";
	}

	@RequestMapping(method=RequestMethod.POST) //처리 결과
	public String submit(OrderCommand orderCommand) {
		System.out.println("orderCommand 주소: " + orderCommand);
		System.out.println("orderCommand 크기: " + orderCommand.getOrderItem().size());

		//검증코드
		List<OrderItem> items = orderCommand.getOrderItem();
		for(OrderItem item: items) {
			System.out.println(item.toString());
		}

		//검증코드
		//model.addAttribute("orderCommand",OrderCommand)
		return "order/OrderCommitted";
	}
}
```

### OrderForm.jsp
```html
<!-- /WEB-INF/views/order/OrderForm.jsp -->
...
<body>
  <form method="post">
<!-- name속성에 배열 사용 가능 -->
  	상품1<br>
  	상품ID: <input type="text" name="orderItem[0].itemid"><br>
  	상품수량: <input type="text" name="orderItem[0].number"><br>
  	상품주의사항:<input type="text" name="orderItem[0].remark"><br>
  	<hr>
  	상품2<br>
  	상품ID: <input type="text" name="orderItem[1].itemid"><br>
  	상품수량: <input type="text" name="orderItem[1].number"><br>
  	상품주의사항:<input type="text" name="orderItem[1].remark"><br>
  	<hr>
  	상품3<br>
  	상품ID: <input type="text" name="orderItem[2].itemid"><br>
  	상품수량: <input type="text" name="orderItem[2].number"><br>
  	상품주의사항:<input type="text" name="orderItem[2].remark"><br>
  	<hr>
  	<input type="submit" value="전송">
  </form>
</body>
...
```

### OrderCommitted.jsp
```html
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>    
...
<body>
	<h3>EL & JSTL 사용 출력</h3>
	<ul>
        <!-- ${orderCommand.orderItem} >> 클래스의 get함수 호출 -->
		<c:forEach items="${orderCommand.orderItem}" var="orderitem">
			<li>
				${orderitem.itemid} / ${orderitem.number} / ${orderitem.remark}
			</li>
		</c:forEach>
	</ul>

</body>
...
```
