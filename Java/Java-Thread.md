# Thread

## Single Thread
지금까지 싱글프로세스 + 싱글쓰레드 (main)함수<br>
JVM >> call stack 하나만 제공<br>
함수 10개 가지고 있어도 현재 실행되는 함수 1개

한번에 하나의 함수만 실행<br>
함수가 [순차적]으로 실행
```java
public class Ex01_single_Thread {
	public static void main(String[] args) {
		System.out.println("나 main 일꾼이야");
		worker();
		worker2();
		System.out.println("나 main 종료");
	}
	static void worker() {
		System.out.println("나 1번 일꾼이야");
	}
	static void worker2() {
		System.out.println("나 2번 일꾼이야");
	}
}
```
