# [JavaScript] Array, JSON

## Array
```html
<!DOCTYPE html>
<html>
<head>
	<meta charset="UTF-8">
	<title>Insert title here</title>
	<script type="text/javascript">
		//java: 정적(배열크기 고정) > int[] arr = {10, 20, 30}
		//java: collection 객체 > ArrayList list = new ArrayList();
		//list.add("A"), list.get ...

		//javascript (Array: 정적, collection)

		var array = ['포도', '사과'];
		document.write(array.toString() + "<br>");

		for(var i = 0; i < array.length; i++) {
			document.write(array[i] + "<br>");
		}

		//array[0] = "포도", array[1] = "사과"
		array[2] = "바나나"; //동적으로 데이터 추가
		document.write(array + " / " + array.length + "<br>");

		array[10] = "애플망고"; //가능
		document.write(array + " / " + array.length + "<br>");

		document.write(array[9] + " / " + array.length + "<br>");
		array[9] = "배";
		document.write("초기화: " + array[9] + " / " + array.length + "<br>");

		var array2 = ["one", "two", "three"];
		document.write(array2.length + "<br>");
		array2.length = 2; //강제로 length
		for(var index in array2) {
			document.write(array2[index] + "<br>");
		}

		array2.length = 4;
		document.write(array2.toString() + "<br>");		

		//POINT
		array2.push("Four");
		document.write("push: " + array2.toString() + "<br>");
		document.write("push: " + array2.length + "<br>");

		//one,two,,,Four
		document.write(array2.pop()); //Four
		document.write(array2.pop()); //undefined
	</script>
</head>
<body>

</body>
</html>
```

## JSON object
```html

<!DOCTYPE html>
<html>
<head>
	<meta charset="UTF-8">
	<title>Insert title here</title>
	<script type="text/javascript">
	/*  
		자바 설계도(클래스) => 재사용성
		class Product{
			private String carname="pony";

			public Product(){}
			public Product(String carname){
				this.carname= carname;
			}
			public void print(){
				System.out.println(this.carname);
			}
		}

		메모리 load ... (new)
		Product p = new Product();
		Product p2 = new Product("pony2");
		p.print();
		p2.print();
		/////////////////////////////////////////////////////
		javaScript >> 객체지향언어(OOP)

		클래스 정의 3가지 방법

		1. 프로토타입 방식 : 일반적인 클래스 제작 방법
                         인스턴스마다 공통된 메서드를 공유해서 사용하는 장점  Jquery 도 prototype 방식으로 설계


		2. 함수 방식 :  간단한 클래스 제작 시 사용
                	인스턴스마다 메서드가 독립적으로 만들어지는 단점


		3. 리터럴 방식  :  클래스 만드는 용도는 아니며 주로 여러개의 매개변수를 그룹으로 묶어 함수의 매개변수로 보낼때
                   	   정의와 함께 인스턴스가 만들어지는 장점이 있음 단 인스턴스는 오직 하나
		   (초보자에게도 중요 ^^)

        4. ECMA6 버전부터 : class 키워드 제공
		    class Person {
		    	  constructor(name) {
		    	    this._name = name;
		    	  }

		    	  sayHi() {
		    	    console.log('Hi! ${this._name}');
		    	  }
		    	}

	[JavaScript 객체 생성]
	1. 오브젝트 리터럴 방식 (객체를 만드는 방법): 클래스 생성과 동시에 객체가 만들어져요
	1.1 리터럴 방식 >> 제일 간단한 방법 > var obj = {}; //var objarr = []; (배열)
	1.2 JSON 표기: {} >> JSON: JavaScript Object Notation
	ex) var myObj = { "name":"John", "age":31, "city":"New York" };
	TIP) JSON >> XML (텍스트 기반의 형식화된 문서 제공)
	XML: 이기종간의 데이터 호환 (한 때는 서점: XML >> webservice 발전)

	다른 이야기
	객체지향언어 장점: 설계도 (재사용성)
	*오브젝트 리터럴 방식: 재사용을 지원하지 않는다
	*설계도를 생성과 동시에 객체 생성(장점: 편하고, 빠르다)
	*설계도를 미리 만들어 놓고 재사용하는 방식은 아니다
	*설계도당 하나의 객체만 생성 사용 (only object)

	예)
	var product = {};
	var product2 = {제품명:'사과', 년도:'2018', 원산지:'대구'};

	var 인스턴스 = {
		프로퍼티: 초기값,
		프로퍼티: 초기값,
		...
		메서드: function() {},
		메서드: function() {}, ...
	}

	리터럴방식: 선언과 동시에 인스턴스가 자동 생성
	var 인스턴스 = {}

	특징: 생성자 존재하지 않는다
		 프로퍼티와 메서드만 정의 가능

	단점: 객체 하나만 생성(재사용성 없다)
	접근방법: 인스턴스이름.자원 >> product2.제품명
	*/

	var product = {제품명:'사과', 년도:'2018', 원산지:'대구'};
	console.log(product);
	document.write(product.제품명 + "<br>");
	document.write(product.원산지 + "<br>");
	document.write(product.년도 + "<br>");
	document.write(product.toString() + "<br>");

	//
	var person = {name:"홍길동",
				  addr:'서울시 강남구 역삼동',
				  eat:function(food) {
					  document.write(this.name + "/" + this.addr + "/" + food + "냠냠");
				  }
			     };
	document.write("<hr>");
	person.eat("apple");
	document.write("<br>" + person.name + "<br>");

	//1.속성 제거
	//var product = {제품명:'사과', 년도:'2018', 원산지:'대구'};
	delete(product.년도);
	var output="";
	/*
		for(var index in Array) {
			arr[index]
		}
	*/
	//아래 for문에서는 in > 객체 > 키값을 return
	//{키:값, 키:값, 키:값}
	for(var key in product) {
		output += "key: " + key + " / " + product[key] + "<br>";
	}
	document.write(output);

	//person 객체도 for문
	for(var key in person) {
		document.write("key: " + key + " / value: " + person[key] + "<br>");
	}

	//json 객체의 활용
	//외부API (우편번호, 서울시 화장실 정보, 날씨정보)
	//JSON 데이터 형태로 받아서 (필요한 정보만 추출해서 사용)

	var Member = {}; //var Member = new Object();
	//빈객체를 만들고 자원 추가
	Member.name = "hong"; //name 속성과 값을 추가
	document.write(Member.name + "<br>");
	Member.age = 100;
	document.write(Member.age + "<br>");

	//필요하다면 함수 추가 가능
	Member.print = function() {
		document.write(this.name + " / " + this.age + "<br>");
	}

	Member.print();

	//JSON 객체 해석하기
	//POINT 속성이 JSON 객체여도 된다.
	var Grade = {
			"list":{
				"kang":10,
				"hong":20,
				"kim":30
			},
			"show":function() {
				for(var key in this.list) {
					document.write(key + ":" + this.list[key] + "<br>");
				}
			}
	}

	Grade.show();
	document.write("<hr>");

	//POINT
	var listobj = Grade.list;
	for(var key in listobj) {
		document.write(key + "," + listobj[key] + "<br>");
	}

	</script>
</head>
<body>

</body>
</html>
```

## Array Object
```html
<!DOCTYPE html>
<html>
<head>
	<meta charset="UTF-8">
	<title>TODAY POINT</title>
	<script type="text/javascript">
		//1. 배열  > []   > var arr = [10,20,30];
		//2. JSON객체  > {} > var ojb = {}

		//배열과 객체 혼용
		var students =[];
		console.log(students);

		//배열에 객체 추가
		//javascript Array (push(), pop() >> stack > LIFO)
		students.push({이름:"홍길동",국어:80,영어:60});
		students.push({이름:"아무개",국어:100,영어:50});
		students.push({이름:"이순신",국어:50,영어:100});


		//기존 객체에 함수 추가
		for(var index in students){
			//var obj = students[index]; //obj json 객체
			students[index].getSum = function(){return this.국어 +this.영어;}
			students[index].getAvg = function(){return this.getSum()/2;}
		}
		console.log(students);
		//문제
		//students 배열에 담겨있는 학생의 이름 , 총점 , 평균을 출력하세요
		var print ="이름&nbsp;&nbsp;&nbsp;총점&nbsp;&nbsp;&nbsp;평균&nbsp;&nbsp;&nbsp;<br>"
		for(var index in students){
			 print+=    students[index].이름 +"&nbsp;&nbsp;";
			 print+=	students[index].getSum() + "&nbsp;&nbsp;";
			 print+=	students[index].getAvg() + "&nbsp;&nbsp;<br>";
		}
	   document.write(print);

	</script>
</head>
<body>

</body>
</html>
```
