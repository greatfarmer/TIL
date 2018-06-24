# [JavaScript] DOM Script

## #1
```html
<!DOCTYPE html>
<html>
<head>
	<meta charset="UTF-8">
	<title>Insert title here</title>
	<script type="text/javascript">
		/*
		When a web page is loaded, the browser creates a Document Object Model of the page.
		The HTML DOM model is constructed as a tree of Objects:

		JavaScript can change all the HTML elements in the page
		JavaScript can change all the HTML attributes in the page
		JavaScript can change all the CSS styles in the page
		JavaScript can remove existing HTML elements and attributes
		JavaScript can add new HTML elements and attributes
		JavaScript can react to all existing HTML events in the page
		JavaScript can create new HTML events in the page

		DOM Script 사용해서 >>위 작업을 수행
		*/

	</script>
</head>
<body>
	<div id="demo"></div>
	<div id="demo">
		두번째 요소 (계층 트리 형태 첫번째 id=demo 찾으면: ...stop)
	</div>
</body>
<script type="text/javascript">
	var ele_div = document.getElementById("demo");
	console.log(ele_div);
	ele_div.innerHTML = "<b>Hello World</b>"; //innerHTML, innerTEXT가 있다.
</script>
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
		//onload 이벤트 발생 시점: body 태그 안에 있는 모든 요소가 브라우저에 loading되고 나서 발생
		//<body onload=call()>
		//window.onload = call() 코드 방식도 적용 가능

		//DOM 처리를 위한 [함수 학습하기]
		//https://www.w3schools.com/js/js_htmldom_document.asp


		window.onload = function() { //함수 이름이 없어요 (익명함수: 일회성): 재활용하지 않겠다
			//page요소가 loading된 후에 함수가 자동 실행되게 하겠다
			//body 안쪽을 제어할 수 있다
			//alert("gogo");
			var out = "";
			out += "<ul>"
				out += "<li>javascript</li>";
				out += "<li>jquery</li>";
				out += "<li>oracle</li>";
			out += "</ul>";
			document.body.innerHTML = out;

			var header = document.createElement("h1"); //<h1></h1>
			var textnode = document.createTextNode("hello");

			header.appendChild(textnode);
			//<h1>hello</h1>

			document.body.appendChild(header);

			//img 태그 생성하고 이미지 src 속성을 만들고 body append
			var img = document.createElement("img"); //<img>
			img.setAttribute("src", "images/1.jpg"); //<img src="images/1.png">
			img.setAttribute("width", '100');
			img.setAttribute("height", '100');
			//<img src="images/1.png" width="100" height="100">
			document.body.appendChild(img);


		}
	</script>
</head>
<body>
	HELLO WORLD
</body>
</html>
```

## #3
```html
<!DOCTYPE html>
<html>
<head>
	<meta charset="UTF-8">
	<title>Insert title here</title>
	<script type="text/javascript">
		window.onload = function() { //function handler
			var pele = document.createElement("p");
			var hrele = document.createElement("hr");
			var h3ele = document.createElement("h3");
			var textnode = document.createTextNode("hello world");
			var divele = document.createElement("div");
			var textnode2 = document.createTextNode("HI HI HI");

			//<hr title="수평선">
			hrele.setAttribute("title", "수평선");

			//<h3>hello world</h3> //기존에 있는 node 자식요소로
			h3ele.appendChild(textnode);

			//<div>HI HI HI</div>
			divele.appendChild(textnode2);

			//body 요소에 추가
			document.body.appendChild(pele).appendChild(divele);
			//<body><p><div>HI HI HI</div></p></body>

			document.body.appendChild(hrele);
			//<body><p><div>HI HI HI</div></p><hr title="수평선"></body>

			document.body.appendChild(h3ele);
			//<body><p><div>HI HI HI</div></p><hr title="수평선"><h3>hello world</h3></body>

		}
	</script>
</head>
<body>

</body>
</html>
```

## #4
```html
<!DOCTYPE html>
<html>
<head>
	<meta charset="UTF-8">
	<title>Insert title here</title>
	<script type="text/javascript">
		/*  
			QUIZ
			window.onload 이벤트에서  btnGetData 요소에 onclick 이벤트를 걸고
			id =txtmessage 요소 객체를 얻어서
			값을(value) console.log() 통해서 출력
			이용 : DOM Script (get....함수 )

			1.window.onload = function(){} //익명함수
			hint) onclick 이벤트 함수 ... 익명함수 방식으로
			      btn.onclick = function(){}

			2.setAttr ...(name, value)
			  getAttr(name)  > return value


		btnCreate 요소에 onclick 이벤트를 걸고
		input 태그를 동적으로 생성하고 (type=text, id=txtbox, value="홍길동") 으로 정의하고
		body 자식요소로 추가하세요

		 */
		window.onload = function() { //onload 이벤트 발생시 익명함수 실행하세요
			var btnobj = document.getElementById("btnGetData");
			btnobj.onclick = function() { //시점 : 버튼을 클릭할때
				var txtmsg = document.getElementById("txtmessage");
				var str = "";
				str += "ID : " + txtmsg.getAttribute("id");
				str += "\nVALUE (초기 value값):" + txtmsg.getAttribute("value");
				str += "\nVALUE (최신 value값) :" + txtmsg.value;
				str += "\nLENGTH :" + txtmsg.value.length;
				console.log(str);
			}

			var btncr = document.getElementById("btnCreate");
			btncr.onclick = function() {
				var inele = document.createElement("input");
				inele.setAttribute("type", "text");
				inele.setAttribute("id", "txtbox");
				inele.setAttribute("value", "홍길동");

				document.body.appendChild(inele);
			}
		}
	</script>
</head>
<body>
	<input type="text" id="txtmessage" value="hello">
	<input type="button" value="getinfo" id="btnGetData">
	<hr>
	<input type="button" value="동적생성" id="btnCreate">
</body>
</html>
```

## #5
```html
<!DOCTYPE html>
<html>
<head>
	<meta charset="UTF-8">
	<title>Insert title here</title>
	<style type="text/css">
		table{width: 100%}
		table,tr,th,td {border: 1px solid; border-collapse: collapse;}
		td{background-color: gold; text-align: center;}
	</style>
	<script type="text/javascript">
		/*
		window.onload =function(){
			document.getElementById("b1").onclick =function(){
				//동적으로 생성되는 테이블
			}
			document.getElementById("b2").onclick =function(){
				//생성된 테이블 삭제
			}
		}
		*/

		function createTable(){
			//동적으로 생성되는 테이블
			/*
			2행 2열 (text 값)
			<table id="tab">
				<tr><td></td><td></td></tr>
				<tr><td></td><td></td></tr>
			</table>

			만들어진 table body > div >  의 자식요소로 추가

			*/
			var intRow = parseInt(document.getElementById("txtrow").value);
			var intColumn = parseInt(document.getElementById("txtcolumn").value);

			var eletable = document.createElement("table");
			eletable.setAttribute("id", "Tab");
			//<table id="Tab"></table>

			for(var i = 0 ; i < intRow ; i++){
				var elerow = document.createElement("tr");
				for(var j = 0 ; j < intColumn ; j++){
					if( i == 0){
						var eCell = document.createElement("th");
						var eText = document.createTextNode((i+1)+"행" + ","+(j+1)+"열");
						eCell.appendChild(eText);
						elerow.appendChild(eCell);
						//<tr><th>1행1열</th></tr>
					}else{
						var eCell = document.createElement("td");
						var eText = document.createTextNode((i+1)+"행" + ","+(j+1)+"열");
						eCell.appendChild(eText); // 추가
						elerow.appendChild(eCell);
					}
				}
				//tr 생성시
				eletable.appendChild(elerow); //Table tr add
			}
			//body
			document.getElementById("div").appendChild(eletable);
		}

		function deleteTable(){
			//생성된 테이블 삭제
			//1. Table id="Tab"
			//같은 id 가진 Table 여러개 있다
			/*
			var tab = document.getElementById("Tab"); //하나의 요소만 가져온다 (제일 먼저 만나는 녀석)
			console.log(tab);
			document.getElementById("div").removeChild(tab);
			*/
			//삭제 (먼저 생성된 것부터 >> 형부터 삭제 >> 같은 ID)
			//id="A"
			//id="A"

			//2. Table id가 없다면
			//document.getElementsByName(name);
			//document.getElementsByClassName(name);
			var tables = document.getElementsByTagName("table"); //page 안에 있는 table 요소 다가
			console.log(tables);
			console.log(tables.length);
			console.log(tables[tables.length - 1]);
			//Array를 return

			//마지막에 생성된 Table 부터 삭제
			if(tables.length > 0) {
				document.getElementById("div").removeChild(tables[tables.length-1])
			}else {
				alert("모두 삭제 되었습니다");
			}
		}
	</script>
</head>
<body>
	<div id="div">
		행의수 : <input type="text" id="txtrow" name="txtrow" value="2"><br>
		열의수 : <input type="text" id="txtcolumn" name="txtcolumn" value="2"><br>
		<input type="button" id="b1" value="동적테이블 생성" onclick="createTable()">
		<input type="button" id="b2" value="동적테이블 제거" onclick="deleteTable()">
	</div>
</body>
</html>
```

## #6
```html
<!DOCTYPE html>
<html>
<head>
	<meta charset="UTF-8">
	<title>Insert title here</title>
	<script type="text/javascript">
	/*
	DOM(성능 조금 떨어지지만 다루기 편해요) <-> SAX(event 기반)(성능 조금 나은데...다루기 어려워요)
	*/
		window.onload = function() {
			var menode;
			menode = document.getElementById("me");
			menode.style.backgroundColor = "yellow"; //css 제어

			var parentnode = menode.parentNode; //body
			parentnode.style.backgroundColor="skyblue";

			var grandnode;
			grandnode = parentnode.parentNode; //html
			grandnode.style.backgroundColor="green";

			var my = document.getElementById("mychild");
			console.log(my);
			console.log(my.firstChild.nodeName); //요소이름 (태그이름)
			console.log(my.firstChild.innerText); //value 속성 없어요
			console.log(my.firstChild.nextSibling.innerText); //형제 Text
			console.log(my.childNodes.length); //모든 LI 요소갯수
			console.log(my.lastChild.innerText);
		}
	</script>
</head>
<body>
<div>A</div><div>B</div><div id="me">C</div><div>D</div><div>E</div>
<ul id="mychild"><li>AA</li><li>BB</li><li>CC</li></ul>
</body>
</html>
```
