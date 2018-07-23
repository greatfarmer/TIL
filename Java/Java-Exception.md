# 예외처리(Exception)

## #1 기본
### 오류
1. 에러(Error): 네트워크 장애, 메모리, 하드웨어
2. 예외(Exception): 개발자 코드 처리 (로직 제어 ...> 예측가능)

예외처리 목적: 프로그램을 정상적으로 수정하는 것이 아니고 문제가 발생시 비정상적인 종료 못하게 하는 것

```java
try{
	문제가 될 수 있는 코드
}catch(Exception e)
{
	처리 (문제에 대한 인지를 하고...)
	ex)관리자에게 메일을 보낼까?
	log파일에 기록할까?
}finally{
	예외가 발생하던 발생하지않던 강제적으로 수행되는 구문
}
```
### 예제
```java
public class Ex01_Exception {
	public static void main(String[] args) {
		System.out.println("Main Start");
			System.out.println("Main Logic 처리");
			try {
				System.out.println (0/0); //비정상 종료 (문제 발생 시점부터 그 이하 코드 실행 안됨)
				//java.lang.ArithmeticException: / by zero
				//new ArithmeticException()
			}catch(Exception e) {
				//처리
				//System.out.println(e.getMessage());
				e.printStackTrace();
			}
		System.out.println("Main End");
	}
}
```

## #2 이중 catch
```java
public class Ex02_Exception {
	public static void main(String[] args) {
		int num = 100;
		int result = 0;

		try {
			for(int i = 0; i < 10; i++) {
				result = num / (int)(Math.random()*10); // 난수 (0~9)
				System.out.println("result: " + result + " i: " + i);
			}
		}catch (ArithmeticException e) {
			System.out.println("연산예외 발생");
		}catch(Exception e) { // 안좋아요..(가독성이...)
			System.out.println("Exception...");
		}
		//연산에 관련된 예외는 ArithmeticException가 잡고 나머지는 Exception 처리
		//하위 예외는 상위(부모) 앞에...
		System.out.println("Main End");
	}
}
```

## #3 finally
```java
import java.io.IOException;

public class Ex03_Exception_finally {
	static void startInstall() {
		System.out.println("INSTALL");
	}
	static void copyFiles() {
		System.out.println("COPY FILES");
	}
	static void fileDelete() {
		System.out.println("DELETE FILES");
	}

	public static void main(String[] args) {
		try {
			copyFiles();
			startInstall(); //설치 중단 되던, 설치 완료 되던 DISK 설치 파일 삭제

			//개발자(사용자) 강제로 예외를 처리
			//사용자 예외 던지기 (예외 객체를 개발자가 직접 생성 new 해라)
			//IOException io = new IOException("Install 처리 중 문제 발생");
			//throw io; //catch가 처리
			throw new IOException("Install 처리 중 문제 발생");
		}catch(Exception e) {
			System.out.println("예외 메시지 출력하기: " + e.getMessage());
		}finally { //예외가 발생하던 발생하지 않던 강제적 실행 블럭
			fileDelete();
		}
		System.out.println("실행");
		//주의 사항
		//**함수 종료 (return;)있다 하더라도 finally 블럭이 있으면 {실행하고}...종료
	}
}
```

## #4 throws
```java
package kr.or.bit;

public class ExClass {
	public ExClass() throws Exception {

	}

	public void call() throws ArithmeticException, IndexOutOfBoundsException {
		System.out.println("call 함수 start");
		int i = 1/0;
		System.out.println("call 함수 end");
	}
}
```

```java
import java.io.FileInputStream;
import java.io.FileNotFoundException;

import kr.or.bit.ExClass;

public class Ex04_Exception_throws {
	public static void main(String[] args) {

		try {
			ExClass ex = new ExClass();
			ex.call();
		} catch (Exception e) {
			e.printStackTrace();
		}

		//클래스 설계시 내가 가지고 있는 자원을 사용하는 개발자에게 강제로 예외처리를 하도록 하는 방법
		//생성자, 함수 뒤에 [throws 예외클래스명, 예외클래스명, 예외클래스명, ...]

		//JAVA API 제공 클래스들은 throws ... (IO)
		//public FileInputStream(String name)
		//throws FileNotFoundException
		/*
		try {
			FileInputStream fi = new FileInputStream("C:\\temp\\a.txt");
		} catch (FileNotFoundException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		*/

	}
}
```
