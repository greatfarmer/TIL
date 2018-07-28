# 4673: 셀프 넘버
> https://www.acmicpc.net/problem/4673

## 문제
셀프 넘버는 1949년 인도 수학자 D.R. Kaprekar가 이름 붙였다. 양의 정수 n에 대해서 d(n)을 n과 n의 각 자리수를 더하는 함수라고 정의하자. 예를 들어, d(75) = 75+7+5 = 87이다.

양의 정수 n이 주어졌을 때, 이 수를 시작해서 n, d(n), d(d(n)), d(d(d(n))), ...과 같은 무한 수열을 만들 수 있다.

예를 들어, 33으로 시작한다면 다음 수는 33 + 3 + 3 = 39이고, 그 다음 수는 39 + 3 + 9 = 51, 다음 수는 51 + 5 + 1 = 57이다. 이런식으로 다음과 같은 수열을 만들 수 있다.

33, 39, 51, 57, 69, 84, 96, 111, 114, 120, 123, 129, 141, ...

n을 d(n)의 생성자라고 한다. 위의 수열에서 33은 39의 생성자이고, 39는 51의 생성자, 51은 57의 생성자이다. 생성자가 한 개보다 많은 경우도 있다. 예를 들어, 101은 생성자가 2개(91과 100) 있다.

생성자가 없는 숫자를 셀프 넘버라고 한다. 100보다 작은 셀프 넘버는 총 13개가 있다. 1, 3, 5, 7, 9, 20, 31, 42, 53, 64, 75, 86, 97

10000보다 작거나 같은 셀프 넘버를 한 줄에 하나씩 출력하는 프로그램을 작성하시오.

## 출력
10,000보다 작거나 같은 셀프 넘버를 한 줄에 하나씩 증가하는 순서로 출력한다.

```
1
3
5
7
9
20
31
42
53
64
 |
 |       <-- a lot more numbers
 |
9903
9914
9925
9927
9938
9949
9960
9971
9982
9993
```

## Java
### Ver 1: ArrayList, 이중 for문 사용 (2018-07-22)
```java
import java.util.ArrayList;
import java.util.List;

public class Main {
	public static int d(int n) {
		String str = String.valueOf(n);

		int sum = n;
		for(int i = 0; i < str.length(); i++) {
			sum += Character.getNumericValue(str.charAt(i));
		}
		return sum;
	}

	public static void main(String[] args) {
		List<Integer> numList = new ArrayList<Integer>();

		for(int i = 0; i <= 10000; i++) {
			int result = d(i);
			numList.add(result);
		}

		boolean check = false;
		for(int i = 0; i <= 10000; i++) {
			check = false;
			for(int j = 0; j <= 10000; j++) {
				if(i == numList.get(j)) check = true;
			}
			if(check == false) System.out.println(i);
		}
	}
}
```

### Ver 2: HashSet, contains 사용 (2018-07-22)
Ver 1의 경우 ArrayList로 숫자가 커질수록 검색속도가 느려짐 (모두 검색하므로) <br>
Ver 2의 경우 HashSet으로 모든 검색하지 않고 Hash알고리즘을 사용하기 때문에 검색속도가 상대적으로 빠름
> https://namu.wiki/w/%ED%95%B4%EC%8B%9C#s-4

```java
import java.util.HashSet;

public class Main {
	public static int d(int n) {
		String str = String.valueOf(n);

		int sum = n;
		for(int i = 0; i < str.length(); i++) {
			sum += Character.getNumericValue(str.charAt(i));
		}
		return sum;
	}

	public static void main(String[] args) {
		HashSet<Integer> numSet = new HashSet<Integer>();

		for(int i = 0; i <= 10000; i++) {
			int result = d(i);
			numSet.add(result);
		}

		for(int i = 0; i <= 10000; i++) {
			boolean exists = numSet.contains(i);
			if(exists == false) System.out.println(i);
		}
	}
}
```
