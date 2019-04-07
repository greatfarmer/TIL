# 2577: 숫자의 개수
> https://www.acmicpc.net/problem/2577

## 문제
세 개의 자연수 A, B, C가 주어질 때 A×B×C를 계산한 결과에 0부터 9까지 각각의 숫자가 몇 번씩 쓰였는지를 구하는 프로그램을 작성하시오.

예를 들어 A = 150, B = 266, C = 427 이라면 

A × B × C = 150 × 266 × 427 = 17037300 이 되고, 

계산한 결과 17037300 에는 0이 3번, 1이 1번, 3이 2번, 7이 2번 쓰였다.

## 입력
첫째 줄에 A, 둘째 줄에 B, 셋째 줄에 C가 주어진다. A, B, C는 모두 100보다 같거나 크고, 1,000보다 작은 자연수이다.

## 출력
첫째 줄에는 A×B×C의 결과에 0 이 몇 번 쓰였는지 출력한다. 마찬가지로 둘째 줄부터 열 번째 줄까지 A×B×C의 결과에 1부터 9까지의 숫자가 각각 몇 번 쓰였는지 차례로 한 줄에 하나씩 출력한다.

## 예제 입력 1
```
150
266
427
```

## 예제 출력 1
```
3
1
0
2
0
0
0
2
0
0
```

## Java (2019-04-07)
```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.HashMap;
import java.util.Map;

public class P2577 {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int[] arr = new int[3];
        for (int i = 0; i < 3; i++) {
            int num = 0;
            do {
                num = Integer.parseInt(br.readLine());
            } while (num < 100 || num >= 1000);
            arr[i] = num;
        }

        print(count(multiply(arr)));
    }

    private static int multiply(int[] arr) {
        int multipliedResult = 1;
        for (int num : arr) {
            multipliedResult *= num;
        }
        return multipliedResult;
    }

    private static Map<Integer, Integer> count(int multipliedResult) {
        String strNum = String.valueOf(multipliedResult);
        Map<Integer, Integer> map = new HashMap<>();

        for (int i = 0; i < strNum.length(); i++) {
            int num = strNum.charAt(i) - 48;
            int count = (map.get(num) == null) ? 0 : map.get(num);
            map.put(num, ++count);
        }

        return map;
    }

    private static void print(Map<Integer, Integer> map) {
        for (int i = 0; i <= 9; i++) {
            int result = (map.get(i) == null) ? 0 : map.get(i);
            System.out.println(result);
        }
    }
}
```
