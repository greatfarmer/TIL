# Collection - Properties
[Properties]<br>
Map 인터페이스 구현한 클래스<br>
특수한 목적: <String, String> 정의

<사용목적><br>
App 공통속성 정의 (환경변수): 프로그램의 버전, 파일경로, 공통변수

<활용><br>
data.properties 파일을 만들어서 (key, value) 구조<br>
DB계정(id, pwd)<br>
버전<br>
다국어<br>

```java
import java.util.Properties;

public class Ex16_Properties {
	public static void main(String[] args) {
		Properties prop = new Properties();
		prop.setProperty("Master", "admin@site.com");
		prop.setProperty("version", "1.0.0.0");
		prop.setProperty("defaultpath", "C:\\Temp\\images");

		System.out.println(prop.getProperty("Master"));
		System.out.println(prop.getProperty("version"));
		System.out.println(prop.getProperty("defaultpath"));
	}
}
```
