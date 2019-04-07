# 2581: 소수
> https://www.acmicpc.net/problem/2581

## 문제
자연수 M과 N이 주어질 때 M이상 N이하의 자연수 중 소수인 것을 모두 골라 이들 소수의 합과 최솟값을 찾는 프로그램을 작성하시오.

예를 들어 M=60, N=100인 경우 60이상 100이하의 자연수 중 소수는 61, 67, 71, 73, 79, 83, 89, 97 총 8개가 있으므로, 이들 소수의 합은 620이고, 최솟값은 61이 된다.

## 입력
입력의 첫째 줄에 M이, 둘째 줄에 N이 주어진다.

M과 N은 10,000이하의 자연수이며, M은 N보다 작거나 같다.

## 출력
M이상 N이하의 자연수 중 소수인 것을 모두 찾아 첫째 줄에 그 합을, 둘째 줄에 그 중 최솟값을 출력한다. 

단, M이상 N이하의 자연수 중 소수가 없을 경우는 첫째 줄에 -1을 출력한다.

## 예제 입력 1
```
60
100
```

## 예제 출력 1
```
620
61
```

## 예제 입력 2
```
64
65
```

## 예제 출력 2
```
-1
```

## Java (2019-04-07)
```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.List;

public class P2581 {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int m, n = 0;
        do {
            m = Integer.parseInt(br.readLine());
        } while (m <= 0 || m > 10000);
        do {
            n = Integer.parseInt(br.readLine());
        } while (n <= 0 || n > 10000 || n < m);

        List<Integer> primeNumbers = getPrimeNumbersInRange(getPrimeList(n), m, n);
        if (primeNumbers.isEmpty()) {
            System.out.println("-1");
        } else {
            System.out.println(sumPrimeNumbers(primeNumbers));
            System.out.println(getMinPrimeNumber(primeNumbers));
        }

    }

    // '에라토스테네스의 체' 구현
   private static List<Boolean> getPrimeList(int n) {
        List<Boolean> primeList = new ArrayList<>(n+1);
        primeList.add(false);
        primeList.add(false);

        for (int i = 2; i <= n; i++) {
            primeList.add(i, true);
        }

        for (int i = 2; (i * i) <= n; i++){
            if (primeList.get(i)){
                for (int j = i * i; j <= n; j += i) {
                    primeList.set(j, false);
                }
            }
        }

        return primeList;
    }

    private static List<Integer> getPrimeNumbersInRange(List<Boolean> primeList, int m, int n) {
        List<Integer> primeNumbers = new ArrayList<>();
        for (int i = m; i <= n; i++) {
            if (primeList.get(i)) primeNumbers.add(i);
        }
        return primeNumbers;
    }

    private static int sumPrimeNumbers(List<Integer> primeNumbers) {
        int sum = 0;
        for (int primeNumber : primeNumbers) {
            sum += primeNumber;
        }
        return sum;
    }

    private static int getMinPrimeNumber(List<Integer> primeNumbers) {
        return primeNumbers.get(0);
    }
}
```