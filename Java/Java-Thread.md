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

## Priority
우선순위: CPU 점유율을 높이겠다 <br>
default: 5 (Max: 10, Min: 1)

```java
class Pth extends Thread {
	@Override
	public void run() {
		for(int i = 0; i < 1000; i++) {
			System.out.println("-----------");
		}
	}
}

class Pth2 extends Thread {
	@Override
	public void run() {
		for(int i = 0; i < 1000; i++) {
			System.out.println("||||||||||");
		}
	}
}

class Pth3 extends Thread {
	@Override
	public void run() {
		for(int i = 0; i < 1000; i++) {
			System.out.println("**********");
		}
	}
}

public class Ex06_priority {
	public static void main(String[] args) {
		Pth ph = new Pth();
		Pth2 ph2 = new Pth2();
		Pth3 ph3 = new Pth3();

		System.out.println(ph.getPriority()); //기본값: 5
		System.out.println(ph2.getPriority());
		System.out.println(ph3.getPriority());

		ph.setPriority(10); // 가장 먼저 끝남
		//ph3.setPriority(1); // 가장 나중에 끝남

		ph.start();
		ph2.start();
		ph3.start();

		System.out.println("Main END...");
	}
}
```

## Daemon Thread
한글 작업 (주작업) ... 보조적으로 (저장) >> Damon ... <br>
Daemon은 주작업과 생명을 같이한다
```java
public class Ex07_Daemon_Thread implements Runnable{
	static boolean autosave = false;

	public static void main(String[] args) {
		Thread th = new Thread(new Ex07_Daemon_Thread());
		th.setDaemon(true);
		th.start();
		//main 하나의 쓰레드 (non-daemon)
		//main의 보조는 th (th는 mian함수의 데몬쓰레드)
		//th는 main과 운명을 같이한다

		for(int i = 0; i <= 30; i++) {
			try {
				Thread.sleep(1000);
			} catch (Exception e) {
				System.out.println(e.getMessage());
			}
			System.out.println("Main Thread: " + i);
			if(i == 5) { //i가 5가 되는 시점에
				System.out.println(i);
				autosave = true;
			}
		}
	}

	@Override
	public void run() {
		while(true) {
			try {
				Thread.sleep(3000);
			} catch (Exception e) {

			}
			if(autosave) { //autosave가 true라면
				autoSave();
			}
		}
	}

	public void autoSave() {
		System.out.println("문서가 3초 간격으로 자동 저장 되었습니다");
	}
}
```

## join()
Thread: state 정보(동작 , 멈춤 ....) <br>
state 정보를 강제: Thread 가지는 생성자 , 함수 <br>
sleep() , join()

복잡한 계산을 여러개의 쓰레드로 나누어서 처리 <br>
그 계산 결과를 참조해서 최종적인 결과를 만들고 싶어요

Thread  동시 처리할때 (누가 먼저 수행될지 알수 없다)

main 함수 Thread, 다른 여러개 Thread <br>
main Thread 가 다른 쓰레드가 끝날때 까지 기다리게 ...

기다리는 녀석이 각각의 Thread 요청 (join())

```java
class Thread_join extends Thread{
	@Override
	public void run() {
		for(int i=0; i < 3000 ;i++) {
			System.out.println("----------");
		}
	}
}

class Thread_join2 extends Thread{
	@Override
	public void run() {
		for(int i=0; i < 3000 ;i++) {
			System.out.println("||||||||||");
		}
	}
}
public class Ex08_ThreadState_Join {

	public static void main(String[] args) {
		Thread_join th = new Thread_join();
		Thread_join2 th2 = new Thread_join2();

		th.start();
		th2.start();

		long starttime = System.currentTimeMillis();

		try {
			  th.join();
			  th2.join();
			  //main 요청 ....
			  //main 나 기다릴거야 th , th2 작업이 끝날때까지
		}catch (Exception e) {
			System.out.println(e.getMessage());
		}

		for(int i = 0 ; i < 3000 ;i++) {
			System.out.println("^^^^^^^^");
		}

		//3개의 쓰레드가 실행한 결과(총 걸린 시간)
		System.out.println("3개의 쓰레드 총 작업 시간 :");
		System.out.println(System.currentTimeMillis());
		System.out.println(starttime);
		System.out.println(System.currentTimeMillis()-starttime);
		System.out.println("Main END");
	}
}
```

## synchronized
동기화 <br>
한강화장실 (공유자원) : 여러명의 사용자가 (10명의 사용자: Thread 10개)

lock <br>
함수 단위 lock -> synchronized

예) 한강 비빔밥 축제 (공유자원) 동시접근 처리
```java
class Wroom {
	public synchronized void openDoor(String name) {
		System.out.println(name + "님 화장실 입장");
		for(int i = 0; i < 100; i++) {
			System.out.println(name + " 사용: " + i);
			if(i == 10) {
				System.out.println(name + "님 끙!!");
			}
		}
		System.out.println("시원하다...");
	}
}

class User extends Thread {
	private Wroom wr;
	private String who;

	public User(String name, Wroom w) {
		this.who = name;
		this.wr = w;
	}

	@Override
	public void run() {
		wr.openDoor(this.who);
	}
}

public class Ex09_Sync_Thread {
	public static void main(String[] args) {
		//한강둔치
		Wroom w = new Wroom();

		//사람들
		User kim = new User("김씨", w);
		User lee = new User("이씨", w);
		User park = new User("박씨", w);

		kim.start(); //stack 할당 받고 run함수 올려놓기
		lee.start();
		park.start();
	}
}
```

## synchronized (cont.)
은행계좌를 하나 가지고 있다 <br>
은행계좌를 통해 입금, 출금 처리를 할 수 있다

친구 5명(똑같은 카드) <br>
동시에 계좌 출금 ('동시'라는 말은 쓰레드를 쓰겠다!의 의미)

통장 1000만원 <br>
ATM 기기에서 동시에 출금

개발자 <br>
누군가 [출금 ~ 행위] 끝날때 까지 LOCK 통해서 자원 보호

```java
class Account{ //계좌
	int balance = 1000; //잔액
	synchronized void withDraw(int money) { //출금
		System.out.println("고객 : " + Thread.currentThread().getName());
		System.out.println("현재 잔액 : " + this.balance);
		if(this.balance >= money) {
			try {
				Thread.sleep(1000); //은행업무 처리되는 과정(인증 ,카드 확인....)
			}catch (Exception e) {
				System.out.println(e.getMessage());
			}
			this.balance -= money;
		}
		System.out.println("인출금액 : " + money);
		System.out.println("인출후 잔액 :" + this.balance);
	}
}

class Bank implements Runnable{
	Account acc = new Account();
	@Override
	public void run() {
		while(acc.balance > 0) {

			int money = ((int)(Math.random()*3)+1)*100;
			//실 출금 처리
			acc.withDraw(money);
		}
	}
}

//class T extends Thread{}

public class Ex10_Sync_Thread {
	public static void main(String[] args) {
		Bank bank = new Bank();

		Thread th = new Thread(bank,"Park");
		Thread th2 = new Thread(bank,"Kim");
		Thread th3 = new Thread(bank,"Lee");

		/* class T extends Thread{}의 경우 Thread의 이름을 설정하는 방법
		T t = new T();
		t.setName("Park");
		*/

		th.start();
		th2.start();
		th3.start();
	}
}
```

## RandomGame
```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.Iterator;
import java.util.List;
import java.util.Map;
import java.util.Scanner;
import java.util.Set;

class TimeCon extends Thread {
	@Override
	public void run() {
		int level = 30;
		try {
			for(int i = level; i > 0; i--) {
				if(P01_RandomGame.count > 6) return;
				System.out.println("남은시간: " + i);
				Thread.sleep(1000);
			}
		}catch(Exception e) {
			System.out.println(e.getMessage());
		}
	}
}

class InputCon extends Thread {
	public static List<Quiz> save;
	@Override
	public void run() {
		QuizInfo questions = new QuizInfo();
		save = new ArrayList<>();
		Scanner sc = new Scanner(System.in);

        for(int i = 0 ; i < questions.quiz.size() ; i++) {
            Set set = questions.quiz.keySet();
            Object[] obj= set.toArray();
            System.out.println(obj[i]); //배열로 해서 문제 출력

            String answer = sc.nextLine(); //쓴답
            String userwrite = questions.quiz.get(obj[i]); //정답
            String result = answer.equals(userwrite) ? "정답" : "오답";
            save.add(new Quiz((String)obj[i], answer, result));
            if(result.equals("정답")) {
            	P01_RandomGame.check++;
            }
            System.out.println(save.get(P01_RandomGame.count).toString());
            P01_RandomGame.count++;
        }
	}
}

class Quiz {
    String question;
    String answer;
    String result;

    public Quiz(String question, String answer, String result) {
        this.question = question;
        this.answer = answer;
        this.result = result;
    }

    public String toString() {
        return  "user가 쓴답 : " + this.answer + "\n채점 : " + this.result;
    }
}

class QuizInfo {
	Map<String, String> quiz;
    public QuizInfo() {
        quiz = new HashMap<>();
        quiz.put("3 x 3 = ", "9");
        quiz.put("6 x 7 = " , "42");
        quiz.put("3 x 9 = ", "27");
        quiz.put("12 x 4 = ", "48");
        quiz.put("15 x 15 = ", "225");
        quiz.put("4 x 14 = ", "56");
        quiz.put("2 x 3 = ", "6");
    }
}

public class P01_RandomGame {
	public static int check;
	public static int count;
	public static void main(String[] args) {
		TimeCon time = new TimeCon();
		InputCon input = new InputCon();

		time.start();
		input.start();

		try {
			time.join();
			input.join();
		}catch (Exception e) {
			System.out.println(e.getMessage());
		}
		System.out.println("==================================================");
		Iterator it = input.save.iterator();
		while(it.hasNext()) {
			System.out.println(it.next().toString());
		}
		System.out.println("==================================================");
		System.out.println("맞춘 문제 수: " + check);
	}
}
```
