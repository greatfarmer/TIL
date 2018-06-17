# XMLHttpRequest

```html
<!DOCTYPE html>
<html>
<head>
 <meta charset="UTF-8">
 <title>Insert title here</title>
 <script type="text/javascript">
 /*
  1. XMLHttpRequest 객체 얻기
  2. onreadystatechange 이벤트 핸들러 구현
  3. 요청 정보 ( open() )
  4. 요청 보내기 (send() )
  5. 응답 처리 (Text(JSON), xml )



   **XMLHttpRequest**
   함수 , 속성 공부

  [ readyState ]
  AJAX 개체의 상태를 나타내는 숫자.
  처음 AJAX 개체를 생성하면 0이다.
  --확인(open())
  get() 메소드로 요청할 페이지 정보를 설정하면 1이 되고,
  send() 메소드로 요청을 보내면 2가 되고,
  서버에서 응답이 오기 시작하면 3이 되고,
  =>서버 응답이 완료되면 4가 된다.
  [ status ]
  서버로부터 받은 [응답의 상태]를 나타내는 숫자.
  =>정상적으로 응답을 받은 경우 200이고,
  페이지를 찾지 못한 경우 404가 된다.
  서버쪽 코드에러 500 이 된다
  [ statusText ]
  서버로부터 받은 응답의 상태를 나타내는 문자열.
  정상적으로 응답을 받으면 'OK'가 되고
  파일을 찾지 못하면 'Not Found'가 된다.
  언제 : XMLHttpRequest 객체 통해서 처리되면 (Server 로 부터의 응답을)

  서버가 응답할 수 있는 자원(client 브라우져에게 응답)
  [ responseText ]
  -서버 응답 내용을 나타내는 문자열(HTML 포함 , JSON)
  [ responseXML ]
  -서버 응답 내용을 나타내는 XML 개체.
  [ onreadystatechange ]
  readyState 속성이 바뀌었을 때 [실행]할 [이벤트 핸들러]를 지정한다.
  onreadystatechange = function(){핸들러 구현 }

  [ open() ]
  open(method, url [, async]);
  AJAX 요청을 초기화하면서 요청 방식, 주소, 동기화 여부를 지정한다.
  method 인자는 http 요청 방식을 나타내며 "get" 또는 "post" 방식을 사용한다.
  url 인자는 요청할 페이지의 주소를 지정한다.
  마지막으로 aysnc 인자는 비동기 통신 여부를 나타내며 true 또는 false로 지정한다.
  aysnc 인자를 지정하지 않으면 true를 기본값[비동기 처리]으로 사용한다.
  ex )

  xhr.open("GET","Ex01_Ajax_resource.html")
  <form method="get" action="Ex01_Ajax_resource.html">
  </form>


  [ send() ]
  send(body);
  AJAX 요청을 보낸다.
  Body 인자에는 요청과 함께 서버로 보낼 내용을 지정한다.
  ex ) xhr.send('');
  <form ....> <input type="submit" > </form>

  [ abort() ]
  abort()
  send() 메소드로 보낸 요청을 취소한다.

  readyState : ajax통신의 진행중인 상태를 알려줍니다.
                 값에 따라 의미하는 상태는 다음과 같습니다.
  0 : 초기화 되지 않은 상태 (open메소드가 아직 호출되지 않은 상태)
  1 : open메소드가 호출된 상태 (send메소드는 호출 되지 않은 상태)
  2 : 송신완료, 요청을 시작한 상태 (요청은 하지 않았지만 데이터가 아직 오지 않은 상태)
  3 : 수신 중인 상태 (데이터가 들어오고 있는 상태)
  4 : 수신 완료 (데이터를 모두 받은 상태)
  Status : 데이터 수신의 성공 여부를 판단해주는 속성입니다 값에 따라 의미하는 상태는 다음과 같습니다
  0 : 로컬로 접근 성공을 의미합니다.
  200 : 해당 url로 접근 성공을 의미합니다.
  403 : 접근이 거부되었음을 의미합니다.
  404 : 해당 url이 없음을 의미합니다.
  500 : 서버오류를 의미합니다.
  responseXML : 받은 데이터를 XML타입으로 변환 시켜줍니다.
  responseText : 받은 데이터를 텍스트 타입으로 변환 시켜줍니다.

  */
  window.onload = function(){
   var xhr;
   if(window.XMLHttpRequest){
    xhr = new XMLHttpRequest(); //모든 브라우져 > XMLHttpRequest
   }else if(window.ActiveXObject){ //IE6 이하
    xhr = new ActiveXObject("Msxml2.XMLHTTP");
   }else{
    throw new Error("AJAX 지원하지 않습니다");
   }
   console.log(xhr);

   //onreadystatechange >> readystate값 변화에 의해서 함수 자동 호출
   xhr.onreadystatechange =function(){ //callback함수
    alert("xhr.readyState :" + xhr.readyState);
       //xhr.readyState (0 ~ 4)
       if(xhr.readyState == 4) //1. 서버에서 응답이 왔다면
     {
        if(xhr.status >= 200  && xhr.status < 300){
         //2.서버에서 응답이 왔고 상태가 (정상 200~299)
         alert("status Text : " + xhr.statusText);

         //정상응답 (서버가 응답한 정상 코드일경우)
         //1.Text > json , html , text.txt
         // > responseText(문자형)
         //2.xml
         // > responseXML (xml 객체)


         document.getElementById("container").innerHTML = xhr.responseText;
        }
    }
   }

   //전송할 준비
   //<form 태그 만드는 것처럼 전송할 준비 action="" method=""
   xhr.open("GET","Ex01_Resource.html"); //<form>  태그 구성
   //전송 하기(submit)
   xhr.send('');

  }
 </script>
</head>
<body>
 <div id="container"></div>
 <img src="images/1.jpg" style="width: 150px;height: 150px">
 <div>HELLO AJAX WORLD</div>
</body>
</html>
```
