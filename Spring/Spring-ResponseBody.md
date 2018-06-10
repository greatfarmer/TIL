# Spring 비동기 처리 (ResponseBody)

## MemberDao Interface
```java
//중복 아이디 체크
public int idCheck(String userid);
```

## MemberDao.xml
```xml
<select id="idCheck" resultType="int">
    select count(*) from member where userid=#{userid}
</select>
```

## JoinController.java
```java
@RequestMapping(value = "idcheck.htm", method = RequestMethod.POST)
public @ResponseBody String idCheck(@RequestBody String userid) {
    MemberDao memberdao = sqlsession.getMapper(MemberDao.class);
    System.out.println(userid.substring(7));

    int result = memberdao.idCheck(userid.substring(7));
    String check = "";
    System.out.println(result);
    if (result > 0) {
        System.out.println("아이디 중복");
        check = "1";
    } else {
        System.out.println("삽입 실패");
        check = "0";
    }
    return check;
}
```

## join.jsp
```html
<script src="https://code.jquery.com/jquery-2.2.4.min.js"></script>

<script type="text/javascript">
    $(document).ready(function(){
        $('#btnCheckUid').click(function() {
            if($('#userid').val()==""){
                alert("아이디를 입력해 주세요")
            }else{
                $.ajax({
                    type : "post",
                    url  : "idcheck.htm",
                    data : {userid:$('#userid').val()},
                    contentType: "application/json; charset=utf-8",
                    success : function(data){
                        console.log(">"+data+"<");
                        if(data.trim() == "1"){
                            alert("사용 불가능한 아이디 입니다.");
                            $('#userid').val("");
                        }else{
                            alert("사용가능한 아이디입니다.");
                        }
                    }
                });

            }
        });
    });
</script>
```

## dispatcher-servlet.xml
```xml
<beans xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation=
                  "http://www.springframework.org/schema/mvc
                   http://www.springframework.org/schema/mvc/spring-mvc.xsd">
<mvc:annotation-driven />
```

## pom.xml
```xml
<!-- jackson  lib -->
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-core</artifactId>
    <version>2.7.3</version>
</dependency>
<dependency>
    <groupId>org.codehaus.jackson</groupId>
    <artifactId>jackson-core-asl</artifactId>
    <version>1.9.13</version>
</dependency>
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.7.3</version>
</dependency>
```
