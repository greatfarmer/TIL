# 1110: 더하기 사이클
> https://www.acmicpc.net/problem/1110

## 문제
0보다 크거나 같고, 99보다 작거나 같은 정수가 주어질 때 다음과 같은 연산을 할 수 있다. 먼저 주어진 수가 10보다 작다면 앞에 0을 붙여 두 자리 수로 만들고, 각 자리의 숫자를 더한다. 그 다음, 주어진 수의 가장 오른쪽 자리 수와 앞에서 구한 합의 가장 오른쪽 자리 수를 이어 붙이면 새로운 수를 만들 수 있다. 다음 예를 보자.

26부터 시작한다. 2+6 = 8이다. 새로운 수는 68이다. 6+8 = 14이다. 새로운 수는 84이다. 8+4 = 12이다. 새로운 수는 42이다. 4+2 = 6이다. 새로운 수는 26이다.

위의 예는 4번만에 원래 수로 돌아올 수 있다. 따라서 26의 사이클의 길이는 4이다.

N이 주어졌을 때, N의 사이클의 길이를 구하는 프로그램을 작성하시오.

## 입력
첫째 줄에 N이 주어진다. N은 0보다 크거나 같고, 99보다 작거나 같은 정수이다.

## 출력
첫째 줄에 N의 사이클 길이를 출력한다.

## 예제 입력 1
```
26
```

## 예제 출력 1
```
4
```

## 예제 입력 2
```
55
```

## 예제 출력 2
```
3
```

## 예제 입력 3
```
1
```

## 예제 출력 3
```
60
```

## Java (2019-04-01)
```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class P1110 {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        int input = 0;
        do {
            input = Integer.parseInt(br.readLine());
        } while (input < 0 || input > 99);

        int cnt = 0;
        int result = input;
        do {
            ++cnt;
            result = calc(result);
        } while (input != result);

        System.out.println(cnt);
    }

    private static int calc(int input) {
        String num = String.valueOf(input);
        int a = (num.length() > 1) ? Integer.parseInt(num.substring(0, 1)) : 0;
        int b = (num.length() > 1) ? Integer.parseInt(num.substring(1, 2)) : Integer.parseInt(num);
        return makeNewNum(a, b);
    }

    private static int makeNewNum(int a, int b) {
        String sum = String.valueOf(a + b);
        String first = String.valueOf(b);
        String last = (sum.length() > 1) ? sum.substring(1, 2) : sum;
        return Integer.parseInt(first + last);
    }
}
```
