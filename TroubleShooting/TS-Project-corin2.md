# TroubleShooting: 코린이 프로젝트(2018-05-21 ~ 2018-07-17)

## GIT
### sourcetree에서 git push시 권한 에러 [2018-06-05]
#### 문제
```
git -c diff.mnemonicprefix=false -c core.quotepath=false push -v --tags --set-upstream origin feature/announce:feature/announce
remote: Permission to corin2/corin2.git denied to jjstudy.
fatal: unable to access 'https://github.com/corin2/corin2.git/': The requested URL returned error: 403

Pushing to https://github.com/corin2/corin2.git
Completed with errors, see above.
```

#### 해결
```
로컬PC의 github키를 삭제(windows 기준)
: 제어판 - 사용자 계정 - 자격 증명 관리 - Windows 자격 증명 - 일반 자격 증명에서 github키 제거

 그 후 push를 해보면 github사용자 계정을 다시 물어봄.
 이 방법의 단점은 로컬PC의 초기 사용자가 아니라면 다시 키 제거를 해야함.
```

### git사용시 파일명.class가 계속 conflict나는 현상 [2018-06-20]
Spring사용시 /target/ 하위 내용들은 컴파일 이후 실행파일들이기 때문에<br>
.gitignore 파일에 추가하여 git commit에서 제외한다.

## MariaDB

### MariaDB에서 오라클의 SYSDATE와 같이 현재 날짜, 시간 입력 [2018-06-04]
```
NOW(), SYSDATE()

[NOW()와 SYSDATE()의 차이]
SELECT * FROM t1 WHERE u_date > NOW()
SELECT * FROM t1 WHERE u_date > SYSDATE()

NOW()와 SYSDATE()의 차이는 쿼리가 길어질 경우 출력 되는 시간이 고정되느냐 변하느냐에 따른 차이가 있습니다.
NOW()는 쿼리가 처음 시작되는 시간이 고정되지만 SYSDATE()는 연산할 때 마다 시간이 변합니다.

예를들어, 애플리케이션에서 1만 건이 되는 유저 정보를 현재 날짜,시간과 함께 조회한다고 가정하겠습니다.
이 때 현재 날짜,시간을 모두 동일하게 출력하고 싶다면 NOW()를 써야 합니다.
즉, 1만 건의 유저 정보가 테이블에 SELECT 되는 동안 컴퓨터가 아무리 빨라도 시간이 걸릴 것이며, 그 때 마다 현재 시간은 변하게 될 것입니다.

NOW()는 쿼리가 시작되는 그 순간의 시간을 고정시키지만,
SYSDATE()는 조회가 이루어지는 row 단위로 시간이 변하게 됩니다.
```
> http://victorydntmd.tistory.com/143

### mariaDB에서 substring 사용하기 [2018-06-23]
```sql
SELECT SUBSTRING_INDEX('admin@corin2.site', '@', -1)
-- 결과 > corin2.site
```

### 정해진 기간 안에 날짜 추가 [2018-06-25]
#### 문제
```
정해진 기간 안에 날짜 추가
예) select ... as days where `date` is between '2018-06-20' and '2018-06-24'

days
----------
2018-06-20
2018-06-21
2018-06-22
2018-06-23
2018-06-24
```
#### 해결
```sql
select * from
(select adddate('1970-01-01',t4*10000 + t3*1000 + t2*100 + t1*10 + t0) gen_date from
 (select 0 t0 union select 1 union select 2 union select 3 union select 4 union select 5 union select 6 union select 7 union select 8 union select 9) t0,
 (select 0 t1 union select 1 union select 2 union select 3 union select 4 union select 5 union select 6 union select 7 union select 8 union select 9) t1,
 (select 0 t2 union select 1 union select 2 union select 3 union select 4 union select 5 union select 6 union select 7 union select 8 union select 9) t2,
 (select 0 t3 union select 1 union select 2 union select 3 union select 4 union select 5 union select 6 union select 7 union select 8 union select 9) t3,
 (select 0 t4 union select 1 union select 2 union select 3 union select 4 union select 5 union select 6 union select 7 union select 8 union select 9) t4) v
where gen_date between '2018-06-20' and '2018-06-24';
```
> https://stackoverflow.com/questions/14105018/generating-a-series-of-dates
<br>http://chan77xx.tistory.com/79

### 오라클> nvl(칼럼명,0)을 mysql로 하면? [2018-06-25]
```sql
mysql> ifnull(칼럼명,0)
```

## UI
### 헤더에서 드롭다운이 짤려 보이는 현상 [2018-06-19]
css 속성에 ```overflow: hidden;```을 작성한 것이 문제였다.<br>
이를 지워줘야 드롭다운이 짤리지 않는다.

### 스크롤을 채팅의 유저리스트div에서만 없애고 싶은 경우 [2018-06-20]
스크롤 기능도 먹히면서, 스크롤바를 안보이게 하는 방법<br>
(원하는 요소에만 적용할 수 있도록, 전체를 하려면 ::-webkit-scrollbar만 쓰면됨)<br>
css에 아래 내용 추가 (원하는 요소::-webkit-scrollbar)
```css
.sideBar::-webkit-scrollbar {
  display:none;
}
```
> https://m.blog.naver.com/PostView.nhn?blogId=fageapp&logNo=220392875038&proxyReferer=https%3A%2F%2Fwww.google.co.kr%2F"a

### 칸반보드가 포함된 페이지에서 채팅 입력 창에 마우스 커서가 나타나지 않는 문제 [2018-07-08]
kanban.js에서 기능 하나하나 주석처리를 하면서 해당 부분을 찾아냄<br>
kanban.js에서 onselectstart='return false' (블럭선택방지)를 주석처리하여 해결
> http://hyunssssss.tistory.com/tag/onselectstart

## JavaScript
### JavaScript에서 함수의 범위 [2018-07-01]

```javascript
$(function() {
  function getChatUsers() {
    ...
  }
});
```
```
위의 getChatUsers함수는 사용범위가 document.ready($function() { ... {)) 내로 한정된다. (지역변수 같이)
이 함수를 다른 document.ready밖의 범위 혹은 다른 script파일에서 사용하고 싶으면 (전역변수 같이)
아래와 같이 함수가 사용되는 document.reday범위 내에서 window.getChatUsers = getChatUsers;를 사용해주면 가능하다.
```
```javascript
$(function() {
  window.getChatUsers = getChatUsers;
  function getChatUsers() {
    ...
  }
});
```

### ajax의 success에서 배열 push()가 되지 않았다. (ajax success밖에서는 동작했다)  [2018-06-23]
ajax success는 비동기 성공시 동작하므로, 이를 동기식으로 변경해주야 한다.<br>
따라서, ajax 옵션에 ```async: false``` 추가하여 동기식으로로 변경
```javascript
$.ajax({
    url: "원하는URL",
    datatype: "JSON",
    async:false,
    success: function(data) {
            // 원하는 코드
    }
});
```
> http://ooz.co.kr/58

## STS
### STS 속도가 느려짐 (코드 한줄 작성하는 것도 버벅임) [2018-06-18]
```
STS에서 window - preferences - General - Show heap status 체크 - OK
했을 때, 화면 하단에 465M of 538M가 나왔다. 이 옆에 휴지통을 눌러줘서 heap memory 정리
```
> http://a-proteur.tistory.com/5 <br>
http://blog.naver.com/PostView.nhn?blogId=ljpark6&logNo=221046462810&categoryNo=26&parentCategoryNo=0&viewDate=&currentPage=1&postListTopCurrentPage=1&from=search

## Spring
### Spring MVC + TOMCAT 실행시 한글처리가 되지 않는 부분 [2018-06-06]
##### [Tomcat 설정]
```xml
<!-- (server.xml) -->
<Connector URIEncoding="UTF-8" connectionTimeout="20000" port="8090"protocol="HTTP/1.1"redirectPort="8443"/>
```

##### [Spring MVC 프로젝트 설정]
```xml
<!-- (web.xml) -->
  <filter>
    <filter-name>EncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
      <param-name>encoding</param-name>
      <param-value>UTF-8</param-value>
    </init-param>
  </filter>
  <filter-mapping>
    <filter-name>EncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
  </filter-mapping>
```

### Spring Security: ${SPRING_SECURITY_LAST_EXCEPTION.message} 값이 Bad conflict 발생 [2018-06-08]
```
password와 id값이 맞지 않아서 권한 에러가 난 것 이다. 쿼리 문을

users-by-username-query="SELECT userid AS username, password, enabled as enabled FROM user WHERE userId= ?"
authorities-by-username-query="select userid, g.GRADENAME as ROLE_NAME from user u JOIN usergrade g on u.gradenum = g.gradenum where u.userId=?"
로 변경을 했더니 인증이 성공했다.

컬럼 명을 신경 써서 mapping해주어야 한다.
```

### 소켓통신을 할때 지정하는 url 매핑에서 200error 발생시 해결하는 법 [2018-06-11]
현재 실행하는 페이지의 url과 같은 값으로 실행했더니 에러가 발생함<br>
그래서 URL의 제일 마지막 부분이 중복이 안되게끔 servlet-context.xml과 URL을 바꾸어 준다.

### pom.xml에 error가 난다면 [2018-06-11]
#### 문제
Multiple annotations found at this line: - <packaging>war</packaging> - web.xml is missing and <failOnMissingWebXml> is set to true

#### 해결
pom.xml의 plugins안에 아래 코드를 추가해준다.
```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-war-plugin</artifactId>
    <version>2.4</version>
    <configuration>
        <warSourceDirectory>src/main/webapp</warSourceDirectory>
            <warName>sample</warName>
        <failOnMissingWebXml>false</failOnMissingWebXml>
    </configuration>
</plugin>
```

### login.html 파일이 프로젝트 run시 불러지지 않는 문제 [2018-06-14]
#### web.xml
```xml
<welcome-file-list>
    <!-- 여기서 /login.html처럼 앞에 /를 넣으면 안된다! -->
    <welcome-file>login.html</welcome-file>
</welcome-file-list>
```

### jsonview 가 정상적으로 작동 하지 않았다. [2018-06-14]
```
pom.xml에 jsonview을 사용할 수 있는 의존 <dependency>가 다른버전으로 2개가 있었다. (중복 잘 확인 할 것)
```

### .classpath 설정 [2018-06-14]
#### 문제
```
Spring 폴더 구조가 site.corin2.controller 처럼 보여야 하나
트러블슈팅의 코드가 .classpath파일에 존재하면
site
└corin2
    └controller
와 같이 보인다.
```

#### 해결
아래의 코드가 .classpath 파일에 존재했기 때문이다.
```xml
<classpathentry excluding="**" kind="src" output="target/test-classes" path="src/test/resources">
    <attributes>
        <attribute name="maven.pomderived" value="true"/>
    </attributes>
</classpathentry>
<classpathentry kind="con" path="org.eclipse.m2e.MAVEN2_CLASSPATH_CONTAINER">
```

### 톰캣 서버 실행하면 HTTP Status 404 – Not Found 에러가 발생
```@RequestMapping("allProjectCount")``` 중복이 존재했다.<br>
코드를 잘 살펴보자!


## UBUNTU
### ubuntu에서 war파일 톰캣폴더로 복사하는 명령어 [2018-06-23]
```
sudo cp corin2-20180623.war /usr/share/tomcat8/webapps
```

### ubuntu에서 tomcat을 완전히 삭제 [2018-07-04]
```
> sudo apt-get purge --auto-remove tomcat8
```
> http://jdlab.tistory.com/305

### [관리자 페이지] - [이메일 관리]에서 vm파일 불러오기가 되지않는 문제 [2018-07-04]
윈도우 환경에서 new FileReader(realpath+"\\signup.vm")를 <br>
우분투 환경에서 new FileReader(realpath+"/signup.vm")로 <br>
'\\' -> '/' 변경함으로 해결

## AWS
### AWS RDS의 mariaDB에서 select * from user;를 하면 table 'user' doesn't exist 에러가 난다 [2018-06-23]
AWS RDS에서는 리눅스에 mariaDB를 설치해 주는데, 리눅스에서는 기본적으로 테이블 대소문자 구별을 한다. <br>
따라서 강제적으로 대소문자 구분하지 않게 하기 위해서는 ```/etc/``` 밑에 있는 mysql 설정파일인 ```my.cnf``` 파일에서 <br>
```lower_case_table_names=1```을 설정해주어야 한다. (AWS RDS에서는 파라미터 그룹에서 설정)
>http://egloos.zum.com/forbis/v/3500411
<br>http://roqkffhwk.tistory.com/91

### AWS RDS DB적용했을 경우, 코린이 사이트에서 로그인이 되지 않는 문제 [2018-06-24]
AWS RDS MariaDB 에서 데이터베이스 Super (root) 권한 행사해주어야 한다. <br>
이는 파라미터그룹에서 ```log_bin_trust_function_creators=1``` 로 설정해주어야 한다.
> http://luckyyowu.tistory.com/316

### AWS RDS MariaDB에 Command Prompt로 접속하기 [2018-06-24]
MariaDB Command Prompt 실행 후
```
> mysql -h corin2mariadb.cat5tvbkgafa.ap-northeast-2.rds.amazonaws.com -P 3306 -u corin2 -p```
입력후, 비밀번호 입력하면 됨

### AWS RDS의 MariaDB에서 insert에서 now()로 현재 시간을 넣으면 현재 오후 5시 10분에 넣었다고 했을 때 2018-06-24 08:08:37 와 같이 북미 시간으로 나온다. [2018-06-24]
AWS RDS의 파라미터그룹에서 time_zone 항목을 Asia/Seoul로 변경하고 RDS 인스턴스를 재시작 해주면 적용된다.
> http://lazyhoneyant.blogspot.com/2016/08/amazon-rds.html


### AWS EC2를 통해 배포 후 corin2.site로 접속했을 때 웹소켓이 동작하지 않음 (아래 오류) [2018-06-26]
#### 문제
```
Error during WebSocket handshake: Unexpected response code: 403
```
#### 해결

크로스도메인 문제 <br>
(ws://호스트IP에서 domain이 corin2.site로 변경되기 때문에 이 도메인에 대한 허용을 해줘야 함)

* 스프링에서 웹소켓 크로스도메인 문제에 대한 참고

As of Spring Framework 4.1.5, the default behavior for WebSocket and SockJS is to accept only same origin requests. It is also possible to allow all or a specified list of origins. This check is mostly designed for browser clients. There is nothing preventing other types of clients from modifying the ```Origin``` header value (see RFC 6454: The Web Origin Concept for more details).

The 3 possible behaviors are:

Allow only same origin requests (default): in this mode, when SockJS is enabled, the Iframe HTTP response header ```X-Frame-Options``` is set to ```SAMEORIGIN```, and JSONP transport is disabled since it does not allow to check the origin of a request. As a consequence, IE6 and IE7 are not supported when this mode is enabled.
Allow a specified list of origins: each provided allowed origin must start with ```http://``` or ```https://```. In this mode, when SockJS is enabled, both IFrame and JSONP based transports are disabled. As a consequence, IE6 through IE9 are not supported when this mode is enabled.
Allow all origins: to enable this mode, you should provide ```*``` as the allowed origin value. In this mode, all transports are available.
WebSocket and SockJS allowed origins can be configured as shown bellow:

> https://docs.spring.io/spring/docs/5.0.0.BUILD-SNAPSHOT/spring-framework-reference/html/websocket.html

아래 코드들을 추가 및 변경

##### WebSocketConfig.java 추가
```java
package site.corin2.ws;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.socket.WebSocketHandler;
import org.springframework.web.socket.config.annotation.EnableWebSocket;
import org.springframework.web.socket.config.annotation.WebSocketConfigurer;
import org.springframework.web.socket.config.annotation.WebSocketHandlerRegistry;

@Configuration
@EnableWebSocket
public class WebSocketConfig implements WebSocketConfigurer{

	@Override
	public void registerWebSocketHandlers(WebSocketHandlerRegistry registry) {
		//TODO 테스트 이후"*" -> "corin.site"로 변경
		registry.addHandler(kanbanHandler(), "/kanbanWebSocket").setAllowedOrigins("*");
		registry.addHandler(msgHandler(), "/msgWebSocket").setAllowedOrigins("*");
		registry.addHandler(headerHandler(), "/headerWebSocket").setAllowedOrigins("*");
	}

	@Bean
	public WebSocketHandler kanbanHandler() {
		return new KanbanHandler();
	}

	@Bean
	public WebSocketHandler msgHandler() {
		return new MsgHandler();
	}

	@Bean
	public WebSocketHandler headerHandler() {
		return new HeaderHandler();
	}

}
```

##### servlet-context.xml 변경
```
<websocket:handlers> -> <websocket:handlers allowed-origins="*">
```
```xml
    ...
	<!-- 모든 사이트 접속: allowed-origins="*"
         특정 사이트 접속: allowed-origins="특정사이트 주소명" -->
	<websocket:handlers allowed-origins="*">
		<websocket:mapping handler="kanbanHandler" path="/kanbanWebSocket" />
		<websocket:handshake-interceptors>
			<bean class="site.corin2.ws.KanbanHandInterceptor"></bean>
		</websocket:handshake-interceptors>
	</websocket:handlers>
	<!-- 메시지소켓 -->
	<websocket:handlers allowed-origins="*">
		<websocket:mapping handler="msgHandler" path="/msgWebSocket" />
		<websocket:handshake-interceptors>
			<bean class="site.corin2.ws.MsgHandInterceptor"></bean>
		</websocket:handshake-interceptors>
	</websocket:handlers>
	<!-- 헤더소켓(멤버제명, 멤버탈퇴, 팀장위임) -->
	<websocket:handlers allowed-origins="*">
		<websocket:mapping handler="headerHandler" path="/headerWebSocket" />
		<websocket:handshake-interceptors>
			<bean class="site.corin2.ws.HeaderHandInterceptor"></bean>
		</websocket:handshake-interceptors>
	</websocket:handlers>
	<bean id="kanbanHandler" class="site.corin2.ws.KanbanHandler" />
	<bean id="msgHandler" class="site.corin2.ws.MsgHandler" />
	<bean id="headerHandler" class="site.corin2.ws.HeaderHandler" />
    ...
```

#### 참고
>(**핵심) 1.  https://docs.spring.io/spring/docs/5.0.0.BUILD-SNAPSHOT/spring-framework-reference/html/websocket.html
<br>2. http://faq.hostway.co.kr/AWS_FAQ/8277
<br>3. https://in1004kyu.github.io/2017/06/14/rails-cable-aws.html
<br>4. https://brunch.co.kr/@adrenalinee31/1
<br>5. https://stackoverflow.com/questions/39627017/error-during-websocket-handshake-unexpected-response-code-403
<br>6. http://egloos.zum.com/namelessja/v/4160294
<br>7. http://syaku.tistory.com/285
<br>8.https://spring.io/guides/gs/rest-service-cors/

### AWS EC2의 ubuntu에 STS에서 maven deploy 실행시 오류 [2018-06-26]
#### 문제
[ERROR] Failed to execute goal org.codehaus.mojo:tomcat-maven-plugin:1.1:deploy (default-cli) on project controller: Cannot invoke Tomcat manager: Error writing to server

#### 해결
ubuntu의 tomcat-users.xml에서 아래와 같이 수정해주어야 한다.
```xml
<tomcat-users>
  <role rolename="manager-gui"/>
  <role rolename="manager-script"/>
  <role rolename="manager-status"/>
  <user username="myManagerId" password="secretP^wd" roles="manager-gui,manager-script,manager-status"/>
</tomcat-users>
```

### AWS EC2의 ubuntu에서 톰캣을 동작시켰고 로그인화면까지 왔다.
하지만 로그인이 되지 않는다. [2018-06-26]
#### 문제
```
http://ec2-52-6-111-163.compute-1.amazonaws.com:8090/corin2/login.html?error=9999
```

#### 해결
AWS RDS에 AWS EC2의 IP를 허용해주지 않았던 문제. RDS의 보안그룹의 인바운드 규칙에 <br>
예) 52.111.22.33/32로 해주었더니 해결되었다.
>http://tbang.tistory.com/53
<br>https://blog.naver.com/carrotcarrot/220671342353

### AWS로 배포했을 때, 웹소켓이 동작하지않음 [2018-06-26]
기존의 ws://localhost:8090/ 을 ws://AWS의 public ip주소:포트번호로 해주어야 동작 - 예)ws://13.125.73.16:8090/
> http://myeonguni.tistory.com/1535

### AWS S3 Key 관리 [2018-06-30]
#### 문제
AWS S3 Key 보안을 위한 .properties 파일 사용법

#### 해결
##### src/main/resources/properties/s3Key.properties
```
//주의사항: 내용에 " "를 붙여주지 않는다.
s3.accessKey = EX0123456789Q
```

##### PropertiesTest.java
```java
package site.corin2.uploadtest;

import org.junit.Test;
import org.springframework.util.ResourceUtils;

import java.io.FileInputStream;
import java.io.IOException;
import java.io.InputStream;
import java.util.Properties;
import java.io.File;


public class PropertiesTest {

    public static Properties fetchProperties(){
        Properties properties = new Properties();
        try {
            // 경로: src/main/resources/properties/파일명.properties
            File file = ResourceUtils.getFile("classpath:properties/s3Key.properties");
            InputStream in = new FileInputStream(file);
            properties.load(in);
        } catch (IOException e) {
            e.printStackTrace();
        }
        return properties;
    }

    @Test
    public void proTest() {
                System.out.println("권한도메인: " + fetchProperties().getProperty("s3.accessKey"));
        }
}
```
> https://stackoverflow.com/questions/9259819/how-to-read-values-from-properties-file

#### 참고 (AWS EC2에서 IAM으로 Key 관리)
> http://jojoldu.tistory.com/300

### S3 access key를 github에 push하지 않아야 한다. [2018-07-04]
실수로라도 S3 access key를 github에 push하면, AWS에서 해당 git repository를 찾은 후 보안상 이유로 계정을 블록하게 된다.<br>
이렇게 되면, 고객센터에 메일을 보내서 계정 블록을 해제해야 하는 수고와 시간이 든다. 보안이 정말 중요하다.

### AWS EC2 ubuntu에서 tomcat 구동 시 느릴 경우 조치 [2018-07-04]
#### [첫번째 방법]
```sudo vi /usr/share/tomcat8/bin/catalina.sh```<br>
catalina.sh을 열어서 '#!/bin/sh' 바로 아래에 옵션 추가<br>
```JAVA_OPTS="$JAVA_OPTS -Djava.security.egd=file:/dev/./urandom"```
#### [두번째 방법]
```cat /proc/sys/kernel/random/entropy_avail```<br>
먼저 패키지를 설치하기 전에 다음 명령어를 통하여 현재 entropy를 확인한다.<br>
결과치가 1000 이하일 경우 haveged를 설치할 것을 권장하고 있다.<br>
```sudo apt-get install haveged```

> http://www.hwangji.kr/sub/dev_leader/link/os/default.aspx?NHBBSID=NHBoardWebTip&NHBBSIDX=74
<br>http://lng1982.tistory.com/261
<br>http://mikelim.mintocean.com/entry/Ubuntu%EC%97%90%EC%84%9C-tomcat%EC%9D%B4-%EB%8A%90%EB%A6%AC%EA%B2%8C-%EB%A1%9C%EB%93%9C-%EB%90%A0-%EB%95%8C

### AWS EC2 ubuntu에서 tomcat 구동 시 handler processing failed; nested exception is java.lang.outofmemoryerror: java heap space 에러 조치 [2018-07-04]
JVM heap memory 용량 부족 에러: ubuntu의 톰캣 설정에서 메모리 설정을 변경<br>
[해결 방법]<br>
```sudo vi /usr/share/tomcat8/bin/catalina.sh```<br>
catalina.sh을 열어서 '#!/bin/sh' 바로 아래에 옵션 추가<br>
```JAVA_OPTS="$JAVA_OPTS -Xms256m -Xmx1024m -XX:MaxPermSize=128m"```

> http://mycup.tistory.com/215
<br>https://okky.kr/article/319932

### STS에서 Maven 배포 시 에러 [2018-07-06]
#### 문제
[ERROR] Failed to execute goal org.codehaus.mojo:tomcat-maven-plugin:1.1:deploy (default-cli) on project controller: Cannot invoke Tomcat manager: Connection refused: connect -> [Help 1]

#### 해결
ubuntu에서 아파치2, 톰캣8을 재시작 한 후 다시 진행 <br>
```sudo service apache2 stop 후 start``` <br>
```sudo service tomcat8 stop 후 start```


### STS에서 Maven 배포 시 에러2 [2018-07-14]
#### 문제
[ERROR] Failed to execute goal org.codehaus.mojo:tomcat-maven-plugin:1.1:deploy (default-cli) on project controller: Cannot invoke Tomcat manager: FAIL - Deployed application at context path /corin2 but context failed to start -> [Help 1]

#### 해결
aws-ec2의 브랜치에서 master를 pull받으면서 src/main/resources/properties/s3Key.properties 파일이 삭제된 것이 문제가 되었다. <br>
이런 상태에서 톰캣 run을 하니 404에러가 발생했었고, 이 파일을 다시 추가하니 메이븐 배포가 정상적으로 완료되었다.

## Firebase
### Java Admin SDK를 사용하여 Firebase의 Realtime Database에 데이터를 입력했을 때, 정상적으로 입력이 되지 않았던 문제 [2018-06-11]

Java 클래스의 main함수를 통해 실행시킬 경우, Firebase의 Realtime DB에 수신되기 전에 main함수가 종료되어 버리는 것이 가장 큰 원인. <br>
덧붙여 Firebase의 리스너는 비동기방식이므로, Firebase 서버와 통신하기위해서 자체 데몬 스레드를 관리해야한다. <br>
따라서, Java 클래스에서는 Firebase의 리스너가 트리거될 때 까지 아래 코드와 같이 latch로 메인 스레드를 차단하여 리스너를 기다리는 방식으로 문제를 해결했다. <br>

> https://stackoverflow.com/questions/44548932/grab-data-from-firebase-with-java
https://stackoverflow.com/questions/46906163/how-to-write-data-to-firebase-with-a-java-program

#### [소스 코드]
```java
package site.corin2.chatting;

import java.io.FileInputStream;
import java.io.IOException;
import java.util.concurrent.CountDownLatch;

import com.google.auth.oauth2.GoogleCredentials;
import com.google.firebase.FirebaseApp;
import com.google.firebase.FirebaseOptions;
import com.google.firebase.database.DataSnapshot;
import com.google.firebase.database.DatabaseError;
import com.google.firebase.database.DatabaseReference;
import com.google.firebase.database.FirebaseDatabase;
import com.google.firebase.database.ValueEventListener;

import site.corin2.model.User;

public class FirebasePut {
    private static final String SERVICE_ACCOUNT_JSON = "D://testcorin2-firebase-adminsdk.json";
    private static final String DATABASE_URL = "https://testcorin2.firebaseio.com/";

    private static DatabaseReference database;

    public static void addUsers() {
            DatabaseReference ref = database.child("user");
            ref.child("user02").setValueAsync((new User("gigi@naver.com", "pic", "hihi")));
    }

    public static void startListeners() {
            final CountDownLatch latch = new CountDownLatch(1);
            database.addValueEventListener(new ValueEventListener() {
                    @Override
                    public void onDataChange(DataSnapshot snapshot) {
                            Object document = snapshot.getValue();
                            System.out.println("받는 문서: " + document);
                            latch.countDown();
                    }
                    @Override
                    public void onCancelled(DatabaseError error) {
                            System.out.println("Error: " + error.getMessage());
                            latch.countDown();
                    }
            });
            try {
                    latch.await();
            } catch (Exception e) {
                    e.printStackTrace();
            }
    }

    public static void main(String[] args) {
    // Initialize Firebase
    try {
            // [START initialize]
            FileInputStream serviceAccount = new FileInputStream(SERVICE_ACCOUNT_JSON);
            FirebaseOptions options = new FirebaseOptions.Builder()
                    .setCredentials(GoogleCredentials.fromStream(serviceAccount))
                .setDatabaseUrl(DATABASE_URL)
                .build();
            FirebaseApp.initializeApp(options);
            // [END initialize]
    } catch (IOException e) {
            e.getStackTrace();
    }

    // Shared Database reference
    database = FirebaseDatabase.getInstance().getReference();

    // Add Users
    addUsers();

    // Start listening to the Database
    startListeners();

    }
}
```

### 채팅목록에서 사용자 두 번 눌러야 채팅 시작되는 문제 [2018-06-16]
#### 문제
```
db.child('privateChats/').once('value', function(snapshot) 시점 문제인 것 같음
```
#### 해결
```
시점이 아래와 같이 데이터를 불러주는 함수 안에서 채팅 출력 함수를 써야 한다.
(이전 코드에서는 아래 함수 밖에 위치했었음)
```
```javascript
db.child('privateChats/').once('value',function(snapshot) {
    messages.on('child_added', showMessage); // DB변동 시 메시지 출력
});
```

### 채팅에서 같은 채팅방을 2번, 3번 눌렀을 때, 2,3번 메시지가 반복해서 출력되는 현상 [2018-06-17]
```javascript
// DB변동 시 메시지 출력 함수
var currentMessages;
function showMessage() {
        $('#conversation').empty(); // 대화창 초기화

        if (currentMessages) {
            // 현재 메시지의 리스너를 off() 함수로 제거해줘야 한다.
            currentMessages.off('child_added', makeMessage);
        }
        messages.on('child_added', makeMessage); // DB변동 시 메시지 출력
        currentMessages = messages;
}
```

### FirebaseDB에서 userid의 문자열 변경 [2018-06-19]
#### 문제
```
 예를 들어 te.st@test.com을 자바스크립트 div의 id값을 사용하기 위해 Uid를 te-st-test-com으로 문자열을 변경했다.
 -> 문제점은 te.st@test.com과 te-st@test.com이 동일한 Uid로 인식한다는 점이다.
 ```
#### 해결
 ```
 이러한 문제점을 해결하기위해, te-st@test.com를 base64로 인코딩을 했으나, dGUtc3RAdGVzdC5jb20= 와 같이 뒤에 =가 생겨서, div의 id값으로 사용하기 부적절했다.
그래서 te-st@test.com를 md5암호화를 이용한 결과, B642B4217B34B1E8D3BD915FC65C4452로 div의 id값으로 적절한 값이 나왔다. 이를 통해 동일한 id가 들어가는 것을 방지했다.
```
##### md5 변환 script (사용 예)
```javascript
<script src="//cdnjs.cloudflare.com/ajax/libs/blueimp-md5/2.10.0/js/md5.min.js"></script>
<script>
var hash = md5("Message");
</script>
 ```
