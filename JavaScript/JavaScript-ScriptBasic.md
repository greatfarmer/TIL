# [JavaScript] ScriptBasic

## #1
```html
<!DOCTYPE html>
<html>
<head>
	<meta charset="UTF-8">
	<title>Insert title here</title>
	<script type="text/javascript">
		/*
			JavaScript 블럭 주석

			JavaScript (언어: Web: 코드해석(웹 브라우저): 라인단위 (해석))
			JavaScript (객체지향언어: 내부적으로 (class 개념))

			JavaScript 사용
			1. HTML의 content, attribute [추가], [변경], [삭제] (동적으로)
			2. CSS의 content [추가], [변경], [삭제] (동적으로)
			3. 유효성 검증 (id체크, 주민번호 검증)
			4. 동적인 웹페이지 구성
			5. 전 세계적으로 1위
				-JavaScript framework >> jquery.js , angular.js , node.js(server) , react.js, vue.js
				-장점: 개발시간의 단축, 유지보수
				-단점: 종속(library) >> framework 만들 수 있는 JavaScript가 우선 학습되어야 한다
			6. 문법
				-1. 대소문자를 엄격하게 구분
				-2. 종결자 (;)
				-3. 타입, 연산자, 제어문 ...
				참고) Java 코드와 유사한 것들이 많음
			7. 사용법 (CSS와 동일: common.css)
				-in-line (태그 안쪽에 표현: <p onclick="<script>..코드..")
				-internal (<head><script>..코드..</head>)
				-external (common.js) -> link 방식으로 사용
		*/

		//internal
		function call() { //java: public void call() { System.out.println(''); }
					alert('internal')
				}
	</script>

	<style type="text/css">
		/* CSS 코드 */
	</style>

	<script type="text/javascript" src="script/common.js"></script>
</head>
<body>
	<h3>in-line</h3>
	<input type="button" value="inline" onclick="alert('inline')">

	<h3>internal</h3>
	<input type="button" value="internal" onclick="call()">

	<h3>external</h3>
	<input type="button" value="external" onclick="excall()">

</body>
</html>
```

## #2
```html
<!DOCTYPE html>
<html>
<head>
	<meta charset="UTF-8">
	<title>Insert title here</title>
	<script type="text/javascript">
		//System.out.println();
		//JavaScript 해석, 결과 출력 (웹 브라우저)
		//웹 브라우저 (해석기 + script API 지원)
		//window 객체 (브라우저 객체 가지고 있고...)
		//window.document 객체
		//document 객체 활용 하면 (화면 출력, 값 처리)
		window.document.write("출력하기<br>");
		document.write("window객체 생략가능<br>");

		console.log("웹브라우저 출력창 제공");
		console.log("웹 브라우저의 cmd 창에 원하는 결과 확인");
		console.log("디버깅, 결과확인, 오류메시지");

		document.write("<br><b>Hello World</b></br>");
		document.write("<table border='1'");
		document.write("<tr><td>AA</td><td>BB</td><tr>");
		document.write("</table>");

		//console.log() 검증 데이터 확인
		console.log(100+200);

		//[JavaScript 순차적으로 실행된다 (순서가 정말 중요하다)]
		//Cannot read property 'elements' of undefined

		//순차적인 line 단우의 해석 (form가 read해서 메모리에 올라와 있어야 하는데...)
		//var element = window.document.forms[0].elements[0];
		//alert(element);

		function data() { //함수의 정보 read 하는데 안에 있는 코드를 생행하지 않음 >> 실행은 호출에 의해서만
			var element = window.document.forms[0].elements[0];
			alert(element);
		}

	</script>
</head>
<body>
	<form action="#" method="get" name="myform">
		ID: <input type="text" name="userid" value="hong"><br>
		PWD: <input type="password" name="pwd">
		<input type="submit" value="전송">
	</form>

	<div style="background-color: gold; width: 200px; height: 150px"
	onmouseover="this.style.backgroundColor='gray'"
	onmouseout="this.style.backgroundColor='gold'">
		현재 javascript 수업중... (this 자기자신: this를 div 태그가 가지고 있어요)
	</div>

	<input type="button" value="클릭해봐" onclick="data()">
</body>
	<script type="text/javascript">
		//시점: form 태그를 read한 후에...
		var element = window.document.forms[0].elements[0];
		console.log(element); //element에 담긴 값은: <input type="text" name="userid" value="hong">
		console.log(element.value);
		console.log(element.value.length);

		//웹 브라우저 객체를 가지고 있다
		//window.alert("정상"); //window는 default이므로 alert만 해도 됨
		//alert(message)

		//객체의 계층...
		//form 태그가 이름을 가지고 있다면 (name="myform")
		//window.document.폼이름.요소이름
		//window.document.폼이름.요소이름.value

		var ele = window.document.myform.userid;
		//ele 변수안에 inputElement 객체가 >> <input type="text" name="userid" value="hong">

		var value = window.document.myform.userid.value;

		console.log("input 태그: " + ele);
		console.log("input 태그 값: " + value);

		if(value.length < 5) {
			alert("다시 입력해");
			ele.value="";
		}

	</script>
</html>
```

## #3
```html
<!DOCTYPE html>
<html>
<head>
	<meta charset="UTF-8">
	<title>DOM(Document Object Model) 모델을 사용하는 JavaScript</title>
	<script type="text/javascript">
		function println() {
			var element = document.getElementById("demo");
			console.log(element);
			element.innerHTML ="HEELO WORLD";
		}
	</script>
</head>
<body>
	<button onclick="println()">클릭하세요</button>

	<button onclick="print()">인쇄</button> <!-- 인쇄화면이 나옴 -->
	<p id="demo">나 문단 태그야</p>
</body>
</html>
```
