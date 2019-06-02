# 1065: 한수
> https://www.acmicpc.net/problem/1065

## 문제
어떤 양의 정수 X의 자리수가 등차수열을 이룬다면, 그 수를 한수라고 한다. 등차수열은 연속된 두 개의 수의 차이가 일정한 수열을 말한다. N이 주어졌을 때, 1보다 크거나 같고, N보다 작거나 같은 한수의 개수를 출력하는 프로그램을 작성하시오.

## 입력
첫째 줄에 1,000보다 작거나 같은 자연수 N이 주어진다.

## 출력
첫째 줄에 1보다 크거나 같고, N보다 작거나 같은 한수의 개수를 출력한다.

## 예제 입력
```
110
```

## 예제 출력
```
99
```

## Java (2018-09-01)
더러운 코드다. 다시 풀어보자
```java
import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

class Hansu {
	int count = 0;

	public void placeNumber(int n) {
		List<Integer> list = new ArrayList<Integer>();

		while (n != 0) {
			list.add(n % 10);
			n /= 10;
		}

		countNumber(list);
	}

	public int run(int n) {
		for (int i = 1; i <= n; i++) {
			placeNumber(i);
		}

		return count;
	}

	public void countNumber(List<Integer> list) {
		int gap = 0;
		int newGap = 0;
		boolean isInit = true;
		boolean isSame = true;

		for (int i = 0; i < list.size(); i++) {
			if (i > 0) {
				newGap = (list.get(i-1)) - (list.get(i));
				if (isInit) {
					gap = newGap;
					isInit = false;
				}
				if (gap != newGap) {
					isSame = false;
				}
			}
		}

		if (isSame) count++;
	}
}

public class P1065_Hansu {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		int input = Integer.parseInt(sc.nextLine());

		Hansu h = new Hansu();

		System.out.println(h.run(input));
	}
}
```
