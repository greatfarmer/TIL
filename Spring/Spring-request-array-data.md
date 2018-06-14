# Spring에서 request array data 처리

> https://blog.seotory.com/post/2016/07/spring-request-array-data

## 기존 방법
스프링에서 가장 흔하게 사용되는 방법 정리

### 단일 데이터
#### html
```html
<input type="text" name="name" value="이름1"/>
<input type="text" name="name" value="이름2"/>
```

#### java controller
```java
@RequestMapping(value = "test", method = RequestMethod.GET)
public ModelAndView test (String[] name) throws Exception {
	for (String str : name) {
		System.out.println(str);
	}
	return null;
}
```
단일 데이터는 정말 어렵지 않다. 생각했던 그대로 데이터가 나온다.

### 다중 배열 처리 방법
#### html
```html
<input type="text" name="name" value="이름1"/>
<input type="text" name="name" value="이름2"/>
<input type="text" name="phone" value="010"/>
<input type="text" name="phone" value="011"/>
```

#### java controller
```java
@RequestMapping(value = "test", method = RequestMethod.GET)
public ModelAndView test (String[] name, String[] phone) throws Exception {
	for (int i=0 ; i<name.length ; i++) {
		System.out.println("name: "+name+", phone: "+phone);
	}
	return null;
}
```
위의 방법은 단일 데이터를 받는 방법에서 조금 더 생각하여 여러가지 파라메터를 받도록 하였다. 하지만 이 방법은 매우 좋지 않다. 일단 각각의 파라메터 배열들의 길이가 달라질 수 있고, 만약 긴 배열을 선택하여 for문을 돌리는 중에 짧은 배열의 over size를 찍게 되면 error를 피할 수 없게 된다.
<br><br>
정말로 이 방법이 사용하고 싶다면, 제일 먼저 가장긴 배열의 size를 체크해야하고, index의 값이 짧은 배열에서 에러가 발생하지 않도록 처리를 해야한다.
<br><br>
사실상 비추천하는 방식이다.

### index를 사용하여 parameter 전달
이번에는 dto를 사용해서 바인딩을 해보자. 스프링에서는 대표적으로 이렇게 사용되고 있다. 먼저는 클라이언트 단에서 parameter를 전달할 때 인덱스 번호를 사용하여 전달 할 수 있다.

#### html
```html
<input type="text" name="list[0].name" value="이름1"/>
<input type="text" name="list[1].name" value="이름2"/>
<input type="text" name="list[0].phone" value="010"/>
<input type="text" name="list[1].phone" value="011"/>
```

#### java dto
```java
public class TestDto {
	private String name;
	private String phone;
	private List<TestDto> list;

	public void setName(String name) {
		this.name = name;
	}

	public String getName() {
		return this.name;
	}

	public void setPhone(String phone) {
		this.phone = phone;
	}

	public String getPhone () {
		return this.phone;
	}

	public void setList(List<TestDto> list) {
		this.list = list;
	}

	public List<TestDto> getList() {
		return this.list;
	}
}
```

#### java controller
```java
@RequestMapping(value = "test", method = RequestMethod.GET)
public ModelAndView test (TestDto dto) throws Exception {
	return null;
}
```

html을 작성하는 방법이 이전이랑 조금 달라지는데 input name의 값이 아예 array의 index 및 객체구조 형태를 포함해야 한다. 또한 dto에서 직접 자기 자신을 리스트로 만든 객체 또한 존재해야한다.
<br><br>
2가지의 불편함에도 불구하고 스프링에서는 이 형태로 가장 많이 사용되고 있다. 문제는 api작성자가 클라이언트단까지 모두 만들면 상관이 없는데, 다른 사람과 협업을 통한 api작업 중엔 form의 input name을 index로 작성해 달라고 요청하면 의아해 할 수도 있다. (스프링을 경험하지 못한 프로그래머라면...)

## @RequestBody를 이용
이번에는 전통적인 parameter를 전달하여 값을 받는 방식 대신에 json string을 활용하여 raw로 데이터를 전송하여 받아보자. 경험상 위의 방법보다 이 방법이 훨씬 간단하였다.

#### html
```html
<input type="text" name="name" value="이름1"/>
<input type="text" name="name" value="이름2"/>
<input type="text" name="phone" value="010"/>
<input type="text" name="phone" value="011"/>
```
@RequestBody를 이용하면 좋은점이 클라이언트 단의 input name을 index와 상관없이 그대로 사용할수 있다는 점이다.

#### java dto
```java
public class TestDto {
	private String name;
	private String phone;
	private List<TestDto> list;

	public void setName(String name) {
		this.name = name;
	}

	public String getName() {
		return this.name;
	}

	public void setPhone(String phone) {
		this.phone = phone;
	}

	public String getPhone () {
		return this.phone;
	}

	public void setList(List<TestDto> list) {
		this.list = list;
	}

	public List<TestDto> getList() {
		return this.list;
	}
}
```
dto는 위와 동일하다.

#### java controller
```java
@RequestMapping(value = "test", method = RequestMethod.GET)
public ModelAndView test (@RequestBody TestDto dto) throws Exception {
	return null;
}
```
#### 호출 예제
```
http://localhost:8080/test?{"list":[{"name":"ssw"},{"name":"ssw2"}]}
```

json string 으로 body부분을 받게 되면, 스프링 컨트롤러 진입전에 자동으로 파싱을 시도하여 json 객체로 만들어 준다. 이 json 객체를 다시 List<TestDto>에 바인딩을 시켜준다.
<br><br>
list wrap 객체를 만드는 것에는 위와 변함이 없지만, html에서 가장 많이 사용되는 객체 구조인 json을 그대로 reqeust로 받아서 처리하는 방식은 input name의 변경이 없고 각각의 파라메터의 길이에 신경쓰지 않아도 되는 장점이 존재한다.

## 결론
제일 마지막에 사용한 @RequestBody를 활용하여 array data를 받게끔 처리하였다. json string raw 데이터로 처리하는 편이 훨신 간편하고 쉬웠다.
