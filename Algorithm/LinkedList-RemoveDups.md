# Linked List 중복값 삭제
> 출처: [엔지니어대한민국 YouTube - [자료구조 알고리즘] Linked List 중복값 삭제 in Java](https://www.youtube.com/watch?v=Ce4baygLMz0)

## Java
```java
class LinkedList {
    LinkedList.Node header;

    static class Node {
        int data;
        LinkedList.Node next = null;
    }

    LinkedList() {
        header = new LinkedList.Node();
    }

    void append(int d) {
        LinkedList.Node end = new LinkedList.Node();
        end.data = d;
        LinkedList.Node n = header;
        while (n.next != null) {
            n = n.next;
        }
        n.next = end;
    }

    void delete(int d) {
        LinkedList.Node n = header;
        while (n.next != null) {
            if (n.next.data == d) {
                n.next = n.next.next;
            } else {
                n = n.next;
            }
        }
    }

    void retrieve() {
        LinkedList.Node n = header.next;
        while (n.next != null) {
            System.out.print(n.data + " -> ");
            n = n.next;
        }
        System.out.println(n.data);
    }

    void removeDups() {
        Node n = header;
        while (n != null && n.next != null) {
            Node r = n;
            while (r.next != null) {
                if (n.data == r.next.data) {
                    r.next = r.next.next;
                } else {
                    r = r.next;
                }
            }
            n = n.next;
        }
    }
}

public class RemoveDups {
    public static void main(String[] args) {
        LinkedList ll = new LinkedList();
        ll.append(2);
        ll.append(1);
        ll.append(2);
        ll.append(3);
        ll.append(4);
        ll.append(4);
        ll.append(2);
        ll.retrieve();
        ll.removeDups();
        ll.retrieve();
    }
}
```

```
2 -> 1 -> 2 -> 3 -> 4 -> 4 -> 2
2 -> 1 -> 3 -> 4
```
