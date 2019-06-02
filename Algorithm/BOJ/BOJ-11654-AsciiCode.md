# 11654: 아스키 코드
> https://www.acmicpc.net/problem/11654

## 문제
알파벳 소문자, 대문자, 숫자 0-9중 하나가 주어졌을 때, 주어진 글자의 아스키 코드값을 출력하는 프로그램을 작성하시오.

## 입력
알파벳 소문자, 대문자, 숫자 0-9 중 하나가 첫째 줄에 주어진다.

## 출력
입력으로 주어진 글자의 아스키 코드 값을 출력한다.

## 예제 입력 / 출력
```
A / 65
C / 67
0 / 48
9 / 57
a / 97
z / 122
```

## Java
### Ver 1: InputStreamReader, BufferedReader 사용 (2018-09-30)
```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class P11654_AsciiCode {
	public static void main(String[] args) throws IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		String input = br.readLine();

		if (input.length() != 1) return; // validation
		char ch = input.charAt(0);

		System.out.println((int)ch);
	}
}
```

### Ver 2: InputStreamReader만 사용 (2018-09-30)
- InputStreamReader의 read()메서드는 char을 하나 읽어 int(ascii코드)로 리턴
- int java.io.InputStreamReader.read() throws IOException
	- Reads a single character.
	- Returns: The character read, or -1 if the end of the stream has been reached.

```java
import java.io.IOException;
import java.io.InputStreamReader;

public class P11654_AsciiCode {
	public static void main(String[] args) throws IOException {
		InputStreamReader isr = new InputStreamReader(System.in);
		System.out.println(isr.read());
	}
}
```
