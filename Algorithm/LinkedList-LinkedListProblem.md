# LinkedListProblem
> 출처: [[자료구조 알고리즘] 단방향 Linked List 뒤부터 세기 in Java](https://www.youtube.com/watch?v=Vb24scNDAVg)

## Java
```java
// 단방향 LinkedList의 끝에서 K번째 노드를 찾는 알고리즘을 구현하시오.

class LinkedList {
    Node header;

    static class Node {
        int data;
        Node next = null;
    }

    LinkedList() {
        header = new Node();
    }

    void append(int d) {
        Node end = new Node();
        end.data = d;
        Node n = header;
        while (n.next != null) {
            n = n.next;
        }
        n.next = end;
    }

    void delete(int d) {
        Node n = header;
        while (n.next != null) {
            if (n.next.data == d) {
                n.next = n.next.next;
            } else {
                n = n.next;
            }
        }
    }

    void retrieve() {
        Node n = header.next;
        while (n.next != null) {
            System.out.print(n.data + " -> ");
            n = n.next;
        }
        System.out.println(n.data);
    }

    Node getFirst() {
        return header.next;
    }
}

public class LinkedListProblem {
    public static void main(String[] args) {
        LinkedList ll = new LinkedList();
        ll.append(1);
        ll.append(2);
        ll.append(3);
        ll.append(4);
        ll.retrieve();

        int k = 2;

        // 방법 1
        LinkedList.Node kth = KthToLastMethod1(ll.getFirst(), k);
        System.out.println("Method 1> Last k(" + k + ")th data is " + kth.data);

        // 방법 2
        Reference r = new Reference();
        LinkedList.Node found = KthToLastMethod2(ll.getFirst(), k, r);
        System.out.println("Method 2> Last k(" + k + ")th data is " + found.data);

        // 방법 3
        LinkedList.Node kthMethod3 = KthToLastMethod3(ll.getFirst(), k);
        System.out.println("Method 3> Last k(" + k + ")th data is " + kthMethod3.data);
    }

    // 방법1
    private static LinkedList.Node KthToLastMethod1(LinkedList.Node first, int k) {
        LinkedList.Node n = first;
        int total = 1;
        while (n.next != null) {
            total++;
            n = n.next;
        }
        n = first;
        for (int i = 1; i < total - k + 1; i++) {
            n = n.next;
        }
        return n;
    }

    static class Reference {
        public int count = 0;
    }

    // 방법2 재귀호출
    private static LinkedList.Node KthToLastMethod2(LinkedList.Node n, int k, Reference r) {
        if (n == null) {
            return null;
        }

        LinkedList.Node found = KthToLastMethod2(n.next, k, r);
        r.count++;
        if (r.count == k) {
            return n;
        }
        return found;
    }

    // 방법3 포인터
    private static LinkedList.Node KthToLastMethod3(LinkedList.Node first, int k) {
        LinkedList.Node p1 = first;
        LinkedList.Node p2 = first;

        for (int i = 0; i < k; i++) {
            if (p1 == null) {
                return null;
            }
            p1 = p1.next;
        }

        while (p1 != null) {
            p1 = p1.next;
            p2 = p2.next;
        }

        return p2;
    }
}

```

```
1 -> 2 -> 3 -> 4
Method 1> Last k(2)th data is 3
Method 2> Last k(2)th data is 3
Method 3> Last k(2)th data is 3
```
