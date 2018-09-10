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

## Multi Thread
Thread: 프로세스에서 하나의 최소 실행단위 (method)

Thread 생성방법
1. Thread 클래스 를 상속 -> class Test extends Thread{} <br>
반드시 Thread 상속할 경우 run()함수 재정의 해야함

2. implements Runnable 구현 -> class Test implements Runnable {} <br>
반드시 run() 추상함수 재정의 <br>
POINT: Thread_2 implements Runnable >> Thread가 아니다, Thread가 될 수 있는 요건만 갖췄다

why 2가지 제공 <br>
class Test extends Car implements Runnable <br>
-> Runnable 인터페이스 사용하는 이유: 다른 상속을 받기 위해서

Thread 추상 클래스 아닌 일반 클래스 <br>
추상클래스라면: new(객체 생성) 할 수가 없음

-> 멀티쓰레드는 동시에 실행되는 것이 아니라 동시에 실행될 수 있는 상황을 만들어주는 것 <br>
start()는 메인함수에 올라가고 새로운 stack에 run()을 올리고 사라진다

```java
class Thread_1 extends Thread {
	@Override
	public void run() { //새로운 stack에 처음 올라가는 함수 (마치 main함수 처럼)
		for(int i = 0; i < 1000; i++) {
			System.out.println("Thread_1: " + i + this.getName());
		}
		System.out.println("Thread_1 run() END");
	}
}

class Thread_2 implements Runnable {
	@Override
	public void run() {
		for(int i = 0; i < 1000; i++) {
			System.out.println("Thread_2: " + i);
		}
		System.out.println("Thread_2 run() END");
	}
}

public class Ex02_Multi_Thread {
	public static void main(String[] args) {
		System.out.println("Main Start");

		//1번
		Thread_1 th = new Thread_1();
		th.start(); //POINT > stack 생성하고 stack run() 올려놓기

		//2번
		Thread_2 th2 = new Thread_2();
		Thread thread = new Thread(th2); //일반 클래스
		thread.start(); //POINT > stack 생성하고 stack run() 올려놓기

		for(int i = 0; i < 1000; i++) {
			System.out.println("main: " + i);
		}
		System.out.println("Main END");
	}
}
```
