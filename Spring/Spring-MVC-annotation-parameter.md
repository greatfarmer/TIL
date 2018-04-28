# Spring MVC Annotation (Cont.)

## TEST_4 Parameter 전송받기

### web.xml
> [Spring MVC Annotation](https://github.com/greatfarmer/TIL/blob/master/Spring/Spring-MVC-annotation.md) 과 동일

### dispatcher-servlet.xml
```xml
<!-- /WEB-INF/dispatcher-servlet.xml -->
...
<!-- TESt_4 설정 parameter  값받기 @RequestParam 활용하기 -->
    <bean class="com.controller.SearchController"></bean>
...
```

### SearchController.java
```java
// /src/com/controller/SearchController.java
package com.controller;

import javax.servlet.http.HttpServletRequest;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.servlet.ModelAndView;

import com.model.Person;

@Controller
public class SearchController {
	//WEB: Client 전송 데이터 -> Server 데이터 받기 (회원가입데이터, 게시판 입력 데이터 등)

	//방법_1: 전통(request 객체)
	// (/search/request.do?query=hong&p=100)
	@RequestMapping("search/request.do")
	public ModelAndView searchRequest(HttpServletRequest request) {
		String query = request.getParameter("query");
		int p = Integer.parseInt(request.getParameter("p"));

		ModelAndView mav = new ModelAndView();
		mav.addObject("query", query);
		mav.addObject("p", p);

		System.out.println("param query: " + query);
		System.out.println("param p: " + p);

		mav.setViewName("search/internal");
		return mav;
	}

    	//[선호도가 두번째로 높음]
	//방법_2: DTO객체를 통해서 받는 방법
    	//선행조건: ?id=kim&name=김유신 >> dto객체의 memberfield 명과 같아야 함
	// (/search/obejct.do?query=hong&p=100)
	@RequestMapping("search/object.do")
	public ModelAndView searchObject(Person person) {
		System.out.println("param query: " + person.getQuery());
		System.out.println("param p: " + person.getP());

		ModelAndView mav = new ModelAndView();
		mav.setViewName("search/internal");
		return mav;
	}

	//방법_3: Spring Annotation > @RequestParam
	//방법_1, 방법_2의 단점: 유효성 체크가 되지 않음
    	//간단한 유효성 처리, 기본값에 대한 설정
	// (/search/internal.do?query=hong&p=100)
	@RequestMapping("search/internal.do")
	public ModelAndView searchInternal(@RequestParam("query") String query,
					   @RequestParam("p") int p) {
		System.out.println("param query: " + query);
		System.out.println("param p: " + p);
		return new ModelAndView("search/internal");
	}

	//방법_3-1: 기본값 설정
	/*
	required: true이면 반드시 필요한 parameter이므로 받지 않으면 error,
	      만약 defaultValue가 있다면 error가 아님
	      false이면 받지 않아도 error가 아님
	*/
	@RequestMapping("search/external.do")
	public ModelAndView searchExternal(@RequestParam(value="query", required=false, defaultValue="default") String query,
					   @RequestParam(value="p", defaultValue="111") int p) {
		System.out.println("param query: " + query);
		System.out.println("param p: " + p);
		return new ModelAndView("search/internal");
	}

 	//[선호도가 가장 높음]
	//방법_4 (편하게: 객체단위로 받지 않는 값들)
	//단 ?query=aaa&p=100 > 함수의 parameter 이름과 동일 > 유효성 처리 안됨
	// (/search/external.do?query=hong&p=100)
	@RequestMapping("search/external.do")
	public ModelAndView searchExternal(String query, int p) {
		System.out.println("param query: " + query);
		System.out.println("param p: " + p);
		return new ModelAndView("search/internal");
	}
}
```
* 방법_5 @PathVariable /member/{memberid}
> 참고: http://hellogk.tistory.com/85

### internal.jsp
```html
<!-- /WEB-INF/views/search/internal.jsp -->
...
<body>
VIEW 페이지: Parameter 처리
</body>
...
```
