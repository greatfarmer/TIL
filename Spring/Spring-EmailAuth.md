# Spring 이메일 인증

## 이메일 중복검사
### Controller
```java
@RequestMapping("EmailCheck.htm")
    public @ResponseBody int EmailCheck(MemberDTO memberdto){
        return memberservice.EmailCheck(memberdto);
    }
```

### Service
```java
public int EmailCheck(MemberDTO memberdto) {
        MemberDAO dao = sqlsession.getMapper(MemberDAO.class);
        return dao.EmailCheck(memberdto);
    }

```

### DAO Interface
```java
// 회원가입시 이메일 중복검사
public int EmailCheck(MemberDTO memberdto);
```

### Mapper
```xml
<select id="EmailCheck" resultType="Integer">
    select count(*) from member where email=#{email}
</select>
```

## 이메일 보내기
### Controller
```java
@RequestMapping("confirm_email.htm")
    public @ResponseBody int sendConfirmEmail(EmailDTO emaildto) throws Exception {
        return emailsender.sendConfirmEmail(emaildto);
    }
```

### Service
```java
public int sendConfirmEmail(EmailDTO emaildto) throws Exception{
        MimeMessage messagedto = mailSender.createMimeMessage();
        MimeMessageHelper messageHelper = new MimeMessageHelper(messagedto, true, "UTF-8");

        Random random = new Random(System.currentTimeMillis());
        int confirmation = 0;

        while(true){
            confirmation = (random.nextInt(10000));
            if(confirmation <10000 && confirmation>1000){break;}
        }

        messageHelper.setFrom("betakosta1@gmail.com"); // 보내는 메일주소는 수정하자 dispatcher-servlet이랑 맞춰주자.        
        messageHelper.setTo(emaildto.getReceiver());
        messageHelper.setSubject("Serendipity : 요청하신 인증번호입니다.");
        messageHelper.setText("요청하신 인증번호는 " + confirmation + "입니다.");

        mailSender.send(messagedto);

        return confirmation;
    }
```

## Client 비동기 처리
```javascript
$.ajax({
        type: "post",
        url : getContextPath()+"/member/EmailCheck.htm",
        data : {"email" : $('#email').val()},
        success : function(data) {
              console.log(data);
              if(data == 1) {
                  swal("이미 같은 메일이 존재합니다!");
                  $('#email').focus();
                  return false;
              } else {
                  swal("입력하신 E-mail 주소로 인증번호가 발송됩니다.");
                  $.ajax({
                        type : "post",
                        url : getContextPath() + "/email/confirm_email.htm",
                        data : {"receiver" : $('#email').val()},
                        success : function(data) {
                            swal("인증번호를 발송했습니다!", "메일을 확인해주세요!")
                            $('#confirm').click(function() {
                                console.log(data);
                                if($('#confirm_number').val() != data) {
                                    swal("인증정보가 정확하지 않습니다!")
                                } else {
                                    swal("인증정보가 확인되었습니다!")
                                    $('#confirm_val').val('1');
                                }
                            })
                        }
                    });
              }
          }
    })
```
