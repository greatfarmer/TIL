# TroubleShooting: 코린이 프로젝트(2018-05-21 ~ 2018-07-17)

## Spring

### AWS EC2를 통해 배포 후 corin2.site로 접속했을 때 웹소켓이 동작하지 않음 (아래 오류)
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
