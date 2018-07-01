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

## Spring

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

## 보안
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
