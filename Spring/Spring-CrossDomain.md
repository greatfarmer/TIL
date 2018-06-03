# Spring에서 Cross Domain 문제 해결 방법
> 출처: http://hutblog.tistory.com/2

CORS 크로스 도메인 이슈 (No 'Access-Control-Allow-Origin' header is present on the requested resource)

## Error 상황

브라우져 console 'Access-Control-Allow-Origin' 문제 발생

나의 경우는 내가 만든 다른 api spring project를 ajax로 받아와 사용하려 하는데 크로스 도메인 이슈가 발생하여 api spring project에 크로스 도메인 문제를 해결하고자 spring 설정을 하게 되었다.

### 1. 크로스 도메인 설정 Java File 만들기

```java
package egovframework.rte.tex.cros;

import java.io.IOException;
import javax.servlet.Filter;
import javax.servlet.FilterChain;
import javax.servlet.FilterConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.http.HttpServletResponse;

public class SimpleCORSFilter  implements Filter {
	public void doFilter(ServletRequest req, ServletResponse res, FilterChain chain) throws IOException, ServletException {
        HttpServletResponse response = (HttpServletResponse) res;
        response.setHeader("Access-Control-Allow-Methods", "POST, GET, OPTIONS, DELETE");
        response.setHeader("Access-Control-Max-Age", "3600");
        response.setHeader("Access-Control-Allow-Headers", "x-requested-with");
        response.setHeader("Access-Control-Allow-Origin", "*");  /*여기의 *을 내가 허용하고 싶은 특정 도메인으로 바꾸면 설정한 도메인에 한에서만 크로스 도메인을 허용하게된다. 여러 도메인의 경우 여러번 설정하면된다. */

        chain.doFilter(req, res);

    }

    public void init(FilterConfig filterConfig) {}

    public void destroy() {}
}
```

### 2. web.xml 필터 설정
```xml
	<filter>
	    <filter-name>cors</filter-name>
	    <filter-class>egovframework.rte.tex.cros.SimpleCORSFilter</filter-class>  <!-- <-- 1번에서 만든 class 위치 -->
	</filter>
	<filter-mapping>
	    <filter-name>cors</filter-name>
	    <url-pattern>/*</url-pattern>  <!-- <-- 전체도메인 해당 (크로스 도메인을 적용하고 싶은 url pattern) -->
	</filter-mapping>
```

## 이 외의 참고 사이트
> https://blog.naver.com/alice_k106/221110159368

> https://jundols.com/2016/03/23/hybrid-app%EC%97%90%EC%84%9C-access-control-allow-origin-%EC%9D%B4%EC%8A%88/

> http://adrenal.tistory.com/16
