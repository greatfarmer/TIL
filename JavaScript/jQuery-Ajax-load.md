# [jQuery] Ajax load

## Ajax_load.jsp

```coffeescript
<%@page import="java.util.Date"%>
<%@page import="java.text.SimpleDateFormat"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
	SimpleDateFormat sdf = new SimpleDateFormat("a hh시 mm분 ss초");
	String msg = request.getParameter("msg");
	String html = "<div>" + msg + sdf.format(new Date().getTime()) + "</div>";
%>
<%= html %>
```

## Ajax_load.html

```html
<!DOCTYPE html>
<html>
<head>
	<meta charset="UTF-8">
	<title>Insert title here</title>
	<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
	<script type="text/javascript">
		/*
			https://www.w3schools.com/jquery/jquery_ref_ajax.asp 참조 페이지
			https://www.w3schools.com/jquery/ajax_load.asp

			Jquery Ajax 작업: xmlHttpRequest 객체를 사용하는 다양한 함수를 제공
			Jquery Ajax: 다양한 비동기 작업을 할 수 있는 함수 제공

			1. Global Ajax Event Handlers
			2. Helper Functions
			3. Low-Level Interface (40%)
			4. Shorthand Methods (50%)

			$(selector).load(url, data, function(response, status, xhr))

			url
				Type: String
				A string containing the URL to which the request is sent.

			data
				Type: PlainObject or String
				A plain object or string that is sent to the server with the request.

			complete
				Type: Function( String responseText, String textStatus, jqXHR jqXHR )
				A callback function that is executed when the request completes.


			load함수
			1. 서버에서 받는 데이터가 (html, text 형식)
			2. client 특정 요소에 삽입을 목적 (innerHTML 기능 포함)

			data > {"msg:hello"} > ?msg=hello
		*/

		$(function() { //load 되면
			$('#btn').click(function() {
				//비동기 함수...
				$('#display').load("Ex01_Ajax_load.jsp",
									{"msg":$('#msg2').val()},
									function(responseText, statusText, xhr) {
										//responseText: 서버가 응답한 결과값 (Text, xml)
										//statusText: 200, 500: 문자리턴 >> success, error
										//xhr: xmlHttpRequest 객체의 주소값 (설정 정보)
										if(statusText == "success") {
											//응답이 왔고 4,, 응답이 정상건이라면 200
											alert('load success: 200 ~ 209: ' + responseText)
										}else {
											//응답이 왔고 4,, 응답이 500, 404 ...
											alert('load fail: ' + xhr.statusText)
										}
									});
			});
		});
	</script>
</head>
<body>
	<h3>동기처리</h3>
	<div id="frmsend">
		<form action="Ex01_Ajax_load.jsp" method="get">
			<input type="text" name="msg" id="msg">
			<input type="submit" value="동기전송">
		</form>
	</div>
	<hr>
	<h3>비동기처리</h3>
	<input type="text" name="msg2" id="msg2">
	<input type="button" value="비동기전송" id="btn">
	<div id="display"></div>
</body>
</html>
```
