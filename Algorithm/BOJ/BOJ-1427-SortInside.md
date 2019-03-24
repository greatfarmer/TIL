# 1427: 소트인사이드
> https://www.acmicpc.net/problem/1427

## 문제
배열을 정렬하는 것은 쉽다. 수가 주어지면, 그 수의 각 자리수를 내림차순으로 정렬해보자.

## 입력
첫째 줄에 정렬하고자하는 수 N이 주어진다. N은 1,000,000,000보다 작거나 같은 자연수이다.

## 출력
첫째 줄에 자리수를 내림차순으로 정렬한 수를 출력한다.

## 예제 입력
```
2143
```

## 예제 출력
```
4321
```

## Java (2019-03-24)
```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String input = br.readLine();
        int[] arr = inputToArr(input);
        selectionSort(arr);
        printArray(arr);
    }

    private static int[] inputToArr(String input) {
        int[] arr = new int[(input.length())];
        for (int i = 0; i < input.length(); i++) {
            arr[i] = (int) input.charAt(i) - 48;
        }
        return arr;
    }

    private static void selectionSort(int[] arr) {
        selectionSort(arr, 0);
    }

    private static void selectionSort(int[] arr, int start) {
        if(start < arr.length - 1) {
            int max_index = start;
            for(int i = start; i < arr.length; i++) {
                if(arr[i] > arr[max_index]) {
                    max_index = i;
                }
            }
            swap(arr, start, max_index);
            selectionSort(arr, start + 1);
        }
    }

    private static void swap (int[] arr, int index1, int index2) {
        int tmp = arr[index1];
        arr[index1] = arr[index2];
        arr[index2] = tmp;
    }

    private static void printArray(int[] arr) {
        for(int data : arr) {
            System.out.print(data);
        }
    }
}
```
