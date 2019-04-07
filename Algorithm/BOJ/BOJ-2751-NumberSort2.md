# 2751: 수 정렬하기 2
> https://www.acmicpc.net/problem/2751

## 문제
N개의 수가 주어졌을 때, 이를 오름차순으로 정렬하는 프로그램을 작성하시오.

## 입력
첫째 줄에 수의 개수 N(1 ≤ N ≤ 1,000,000)이 주어진다. 둘째 줄부터 N개의 줄에는 숫자가 주어진다. 이 수는 절댓값이 1,000,000보다 작거나 같은 정수이다. 수는 중복되지 않는다.

## 출력
첫째 줄부터 N개의 줄에 오름차순으로 정렬한 결과를 한 줄에 하나씩 출력한다.

## 예제 입력
```
5
5
4
3
2
1
```

## 예제 출력
```
1
2
3
4
5
```

## Java (2019-04-01)
```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.List;

public class P2751 {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int n = Integer.parseInt(br.readLine());
        List<Integer> list = new ArrayList<>(n);
        while (list.size() < n) {
            int number = Integer.parseInt(br.readLine());
            list.add(number);
        }

        ascSort(list, 0);
        print(list);
    }

    private static void ascSort(List<Integer> list, int currIndex) {
        if (currIndex == list.size()) return;
        int minIndex = currIndex;
        for (int i = currIndex; i < list.size(); i++) {
            minIndex = (list.get(i) < list.get(minIndex)) ? i : minIndex;
        }
        swap(list, currIndex, minIndex);
        ascSort(list, ++currIndex);
    }

    private static void swap(List<Integer> list, int a, int b) {
        int temp = list.get(a);
        list.set(a, list.get(b));
        list.set(b, temp);
    }

    private static void print(List<Integer> list) {
        for (int num : list) {
            System.out.println(num);
        }
    }
}
```
