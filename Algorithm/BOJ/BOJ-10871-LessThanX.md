# 10871: X보다 작은 수
> https://www.acmicpc.net/problem/10871

## 문제
정수 N개로 이루어진 수열 A와 정수 X가 주어진다. 이때, A에서 X보다 작은 수를 모두 출력하는 프로그램을 작성하시오.

## 입력
첫째 줄에 N과 X가 주어진다. (1 ≤ N, X ≤ 10,000) <br>
둘째 줄에 수열 A를 이루는 정수 N개가 주어진다. 주어지는 정수는 모두 1보다 크거나 같고, 10,000보다 작거나 같은 정수이다.

## 출력
X보다 작은 수를 입력받은 순서대로 공백으로 구분해 출력한다. X보다 작은 수는 적어도 하나 존재한다.

## 예제 입력
```
10 5
1 10 4 9 2 3 8 5 7 6
```

## 예제 출력
```
1 4 2 3
```

## Java (2018-09-16)
중복에 대한 예외처리가 미흡하다
```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.List;

class LessClass {
	public boolean exceptionProcess(String[] inputArr) {
		for (String input : inputArr) {
			int parseInt = Integer.parseInt(input);
			if (parseInt < 1 || parseInt > 10000) {
				return true;
			}
		}
		return false;
	}

	public List<Integer> findLessNum(int x, List<Integer> a) {
		List<Integer> numList = new ArrayList<>();
		for (int i : a) {
			if (i < x) {
				numList.add(i);
			}
		}
		return numList;
	}
}

public class P10871_LessThanX {
	public static void main(String[] args) throws IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		String[] inputFirst = br.readLine().split(" ");
		String[] inputSecond = br.readLine().split(" ");

		LessClass lessClass = new LessClass();
		boolean isException = false;

		// 예외처리
		isException = lessClass.exceptionProcess(inputFirst);
		isException = lessClass.exceptionProcess(inputSecond);
		if (isException) return;

		// N, X 설정
		int n = Integer.parseInt(inputFirst[0]);
		int x = Integer.parseInt(inputFirst[1]);

		// 수열 A 설정
		List<Integer> a = new ArrayList<>();
		for (String input : inputSecond) {
			a.add(Integer.parseInt(input));
		}
		if (a.size() > n ) return; // 예외처리

		// X보다 작은 수 처리
		List<Integer> result = lessClass.findLessNum(x, a);

		// 결과 출력
		for (int num : result) {
			System.out.print(num + " ");
		}
	}
}
```
