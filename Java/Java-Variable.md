# 변수(Variable)

## 변수 Scope (유효범위): 선언되는 위치에 따라서
1. instance variable: 객체변수(전역변수) class Test { ... }
2. local variable   : 지역변수 (함수 안에 있는 변수) class Test { void run() { ... } }
3. static variable  : 공유변수

## 클래스 생성
1. 설계도 용도 만드는 클래스: kr.co.pat
2. 실행을 위해 만드는 클래스 (실행점: main() 함수): Ex01_variable
3. 함수 public static void main(String[] args) {}: 프로그램 시작점, 진입점 <br>
TIP) C# > public static void Main()

## 예제
```java
// 설계도 == class
class Test {
	int iv = 500;
	// instance variable: Heap 메모리에 올라가 있는 한 프로그램 종료될 때 까지 사용 가능
	// 이 변수는 초기화를 하지 않아도 된다 (기본값: default를 가지고 있다.)
	String sv; // instance variable

	void print() {
		int lv = 100; // local variable
	// 함수가 호출되면 그때 메모리에 생성되고 함수가 종료되면 메모리에서 사라진다.
	}
	void write() {
		System.out.println("iv: 전역변수: " + iv);
	}
}
```
```java
public class Ex01_variable {
	public static void main(String[] args) {
		int lv = 500; // local variable
		System.out.println("lv변수값: " + lv); // 출력하는 함수

		// int number; //선언
		// System.out.println(number);
		// The local variable number may not have been initialized

		int number = 100;
		System.out.println("초기화: " + number);
		// 1.지역변수는 반드시 [초기화]하고 사용하여야 한다. (처음 값을 입력하는 것을 초기화)
		// 2.약속: 함수 안에 있는 지역변수(local variable)은 반드시 초기화하자

		int num; //선언만 하면 문제 x
		num = 200;
		System.out.println(num);
	}
}
```
