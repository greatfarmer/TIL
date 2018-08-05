# 10845: 큐
> https://www.acmicpc.net/problem/10845

## 문제
정수를 저장하는 큐를 구현한 다음, 입력으로 주어지는 명령을 처리하는 프로그램을 작성하시오.

명령은 총 여섯 가지이다.

- push X: 정수 X를 큐에 넣는 연산이다.
- pop: 큐에서 가장 앞에 있는 정수를 빼고, 그 수를 출력한다. 만약 큐에 들어있는 정수가 없는 경우에는 -1을 출력한다.
- size: 큐에 들어있는 정수의 개수를 출력한다.
- empty: 큐가 비어있으면 1, 아니면 0을 출력한다.
- front: 큐의 가장 앞에 있는 정수를 출력한다. 만약 큐에 들어있는 정수가 없는 경우에는 -1을 출력한다.
- back: 큐의 가장 뒤에 있는 정수를 출력한다. 만약 큐에 들어있는 정수가 없는 경우에는 -1을 출력한다.

## 입력
첫째 줄에 주어지는 명령의 수 N (1 ≤ N ≤ 10,000)이 주어진다. 둘째 줄부터 N개의 줄에는 명령이 하나씩 주어진다. 주어지는 정수는 1보다 크거나 같고, 100,000보다 작거나 같다. 문제에 나와있지 않은 명령이 주어지는 경우는 없다.

## 출력
출력해야하는 명령이 주어질 때마다, 한 줄에 하나씩 출력한다.

## 예제 입력
```
15
push 1
push 2
front
back
size
empty
pop
pop
pop
size
empty
pop
push 3
empty
front
```

## 예제 출력
```
1
2
2
0
1
2
-1
0
1
-1
0
3
```

## Java (2018-08-05)
```java
import java.util.Scanner;

class Queue {
	class Node {
		private int data;
		private Node next;

		public Node(int data) {
			this.data = data;
		}
	}

	private Node first;
	private Node last;
	private int cnt = 0;

	public void push(int item) {
		Node n = new Node(item);

		if(last != null) {
			last.next = n;
		}
		last = n;

		if(first == null) {
			first = last;
		}

		cnt++;
	}

	public int pop() {
		if(first == null) return -1;

		int item = first.data;
		first = first.next;

		cnt--;
		return item;
	}

	public int size() {
		return cnt;
	}

	public int empty() {
		return (first == null) ? 1 : 0;
	}

	public int front() {
		if(first == null) return -1;
		return first.data;
	}

	public int back() {
		if(first == null) return -1;
		return last.data;
	}
}

public class Main {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		int initNum = Integer.parseInt(sc.nextLine());
		Queue q = new Queue();

		int i = 0;
		while(i < initNum) {
			String input = sc.nextLine();
			if(input.contains("push")) {
				int x = Integer.parseInt(input.substring(5));
				q.push(x);
			}else if(input.contains("pop")) {
				System.out.println(q.pop());
			}else if(input.contains("size")) {
				System.out.println(q.size());
			}else if(input.contains("empty")) {
				System.out.println(q.empty());
			}else if(input.contains("front")) {
				System.out.println(q.front());
			}else if(input.contains("back")) {
				System.out.println(q.back());
			}else {
				continue;
			}

			i++;
		}		
	}
}
```
