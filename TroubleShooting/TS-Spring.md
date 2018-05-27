# TroubleShooting: Spring

### 공공데이터 API 사용
문제
```
공공 데이터 API를 json으로 가져올 때, 1회 1000건만 가능
-> 전체 데이터를 표시하고 싶은데, 1000건으로 제한되는 문제
```
해결
```javascript
기존: var apiUrl = "http://openapi.seoul.go.kr:8088/61684e6a7573756e38316e6f504b69/json/PublicWiFiPlaceInfo/1/1000";
수정: var apiUrl = [];
        apiUrl.push("http://openapi.seoul.go.kr:8088/61684e6a7573756e38316e6f504b69/json/PublicWiFiPlaceInfo/1/1000");
        apiUrl.push("http://openapi.seoul.go.kr:8088/61684e6a7573756e38316e6f504b69/json/PublicWiFiPlaceInfo/1001/2000");
        apiUrl.push("http://openapi.seoul.go.kr:8088/61684e6a7573756e38316e6f504b69/json/PublicWiFiPlaceInfo/2001/3000");

url값을 배열로 만들어 주어, 아래와 같이 $.each로 데이터를 모두 표시해 주어 해결.

$.each(apiUrl, function(urlIndex, urlObj) {
                $.ajax({
                   url : urlObj,
                   dataType:"json",
                   success:function(data){ ...
```

### 2차 프로젝트 문제
문제
```
1. 카드의 cardNum 값을 상세페이지 Modal창에 넘기지 못하는 문제
2. fullcalendar에서 종료일 end: '2018-04-10' 으로 하면 달력에는
2018-04-09까지 표시되고 editable이 가능하며,
end: '2018-04-10T23::59:59' 로 하면 달력에는 
2018-04-10까지 표시되나 editable이 불가능한 문제
```
해결
```
<input type="hidden" id=cardNum>을 사용하여 태그에 id를 숨겨서 cardNum값 넘김
```

### Fullcalendar
문제
```
fullcalendar에서 일정 추가시 datepicker에서
받은 종료일이 하루 모자르게 표시되는 부분
```
해결
```
function dateProcess(dateStr, days){ //date가공 메서드
                                var date = new Date(dateStr); //Date 객체 생성
                                date.setDate(date.getDate() + days); //Date 가감설정
                                var dateToISO = date.toISOString().substring(0,10); //Date -> String (ISO날짜 타입, 시간제거)
                                return dateToISO;
                        }
위의 함수를 사용하여 실제날짜, 화면에 출력되는 날짜의 원하는 date로 설정함
```

### MyBatis 오류
문제
```
심각: Servlet.service() for servlet [appServlet] in context with path [/kang] threw exception [Request processing failed; nested exception is org.apache.ibatis.binding.BindingException: Type interface kr.co.kang.dao.UserDao is not known to the MapperRegistry.] with root cause
org.apache.ibatis.binding.BindingException: Type interface kr.co.kang.dao.UserDao is not known to the MapperRegistry.
```

해결
```

```
