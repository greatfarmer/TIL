# 예외처리(Exception)

## #1
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
