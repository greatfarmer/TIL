# 10866: 덱
> https://www.acmicpc.net/problem/9375

## 문제
정수를 저장하는 덱(Deque)를 구현한 다음, 입력으로 주어지는 명령을 처리하는 프로그램을 작성하시오.

명령은 총 여덟 가지이다.
- push_front X: 정수 X를 덱의 앞에 넣는다.
- push_back X: 정수 X를 덱의 뒤에 넣는다.
- pop_front: 덱의 가장 앞에 있는 수를 빼고, 그 수를 출력한다. 만약, 덱에 들어있는 정수가 없는 경우에는 -1을 출력한다.
- pop_back: 덱의 가장 뒤에 있는 수를 빼고, 그 수를 출력한다. 만약, 덱에 들어있는 정수가 없는 경우에는 -1을 출력한다.
- size: 덱에 들어있는 정수의 개수를 출력한다.
- empty: 덱이 비어있으면 1을, 아니면 0을 출력한다.
- front: 덱의 가장 앞에 있는 정수를 출력한다. 만약 덱에 들어있는 정수가 없는 경우에는 -1을 출력한다.
- back: 덱의 가장 뒤에 있는 정수를 출력한다. 만약 덱에 들어있는 정수가 없는 경우에는 -1을 출력한다.

## 입력
첫째 줄에 주어지는 명령의 수 N (1 ≤ N ≤ 10,000)이 주어진다. 둘쨰 줄부터 N개의 줄에는 명령이 하나씩 주어진다. 주어지는 정수는 1보다 크거나 같고, 100,000보다 작거나 같다. 문제에 나와있지 않은 명령이 주어지는 경우는 없다.

## 출력
출력해야하는 명령이 주어질 때마다, 한 줄에 하나씩 출력한다.

## 예제 입력 1
```
15
push_back 1
push_front 2
front
back
size
empty
pop_front
pop_back
pop_front
size
empty
pop_back
push_front 3
empty
front
```

## 예제 출력 1
```
2
1
2
0
2
1
-1
0
1
-1
0
3
```

## 예제 입력 2
```
22
front
back
pop_front
pop_back
push_front 1
front
pop_back
push_back 2
back
pop_front
push_front 10
push_front 333
front
back
pop_back
pop_back
push_back 20
push_back 1234
front
back
pop_back
pop_back
```

## 예제 출력 2
```
-1
-1
-1
-1
1
1
2
2
333
10
10
333
20
1234
1234
20
```

## Java (2019-06-02)
```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.ArrayDeque;
import java.util.Deque;

public class P10866 {
    static BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    static BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

    public static void main(String[] args) throws IOException {
        run(br, bw);
        bw.flush();
        br.close();
        bw.close();
    }

    public static void run(BufferedReader br, BufferedWriter bw) throws IOException {
        int n = Integer.parseInt(br.readLine());
        Deque<Integer> deque = new ArrayDeque<>();
        for (int i = 0; i < n; i++) {
            String[] input = br.readLine().split("\\s");
            if (input.length == 2) {
                push(deque, input);
            } else if (input[0].contains("pop_")) {
                bw.write(pop(deque, input[0]) + "\n");
            } else if ("size".equals(input[0])) {
                bw.write(size(deque) + "\n");
            } else if ("empty".equals(input[0])) {
                bw.write(empty(deque) + "\n");
            } else if ("front".equals(input[0])) {
                bw.write(front(deque) + "\n");
            } else if ("back".equals(input[0])) {
                bw.write(back(deque) + "\n");
            }
        }
    }

    public static void push(Deque<Integer> deque, String[] input) {
        int x = Integer.parseInt(input[1]);
        switch (input[0]) {
            case "push_front":
                deque.offerFirst(x);
                break;
            case "push_back":
                deque.offerLast(x);
                break;
            default:
                break;
        }
    }

    public static int pop(Deque<Integer> deque, String input) {
        switch (input) {
            case "pop_front":
                Object result = deque.pollFirst();
                return (result != null) ? (int) result : -1;
            case "pop_back":
                result = deque.pollLast();
                return (result != null) ? (int) result : -1;
            default:
                return -1;
        }
    }

    public static int size(Deque<Integer> deque) {
        return deque.size();
    }

    public static int empty(Deque<Integer> deque) {
        return deque.isEmpty() ? 1 : 0;
    }

    public static int front(Deque<Integer> deque) {
        Object result = deque.peekFirst();
        return (result != null) ? (int) result : -1;
    }

    public static int back(Deque<Integer> deque) {
        Object result = deque.peekLast();
        return (result != null) ? (int) result : -1;
    }
}
```
