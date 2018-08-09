# Collection - Iterator
나열된 자원에 대해 순차적으로 접근해서 값을 리턴하는 표준 정의<br>
Iterator 인터페이스<br>
```hasNext()```, ```Next()``` => 자원을 ArrayList 구현하고 있음<br>
* ```Iterator``` 순방향 데이터 read하는 표준
* ```ListIterator``` 양방향 데이터 read하는 표준 (순방향으로 읽은 후, 역방향으로 읽음)

```java
import java.util.ArrayList;
import java.util.Iterator;
import java.util.ListIterator;

public class Ex09_Collection_Iterator {
	public static void main(String[] args) {
		ArrayList list = new ArrayList();
		list.add(100);
		list.add(200);
		list.add(300);
		for(int i = 0; i < list.size(); i++) {
			System.out.println(list.get(i));
		}

		//표준화된 인터페이스 사용해서 출력한다면
		//Iterator
		//Collection { Iterator iterator(); } //추상 메서드
		//List extends Collection
		//ArrayList implements List
		//ArrayList는 iterator(){} 추상 메서드 강제 구현
		//부모 타입인 Iterator 로 접근 Override된 ArrayList의 iterator 함수 호출

		Iterator it = list.iterator();
		while(it.hasNext()) {
			System.out.println("[" + it.next() + "]");
		}

		//generic
		ArrayList<Integer> intlist = new ArrayList<>();
		intlist.add(44);
		intlist.add(55);
		intlist.add(66);

		Iterator<Integer> list2 = intlist.iterator();
		while(list2.hasNext()) {
			System.out.println("[" + list2.next() + "");
		}

		//역방향
		for(int i = intlist.size()-1; i >= 0; i--) {
			System.out.println(intlist.get(i));
		}

		//ListIterator (순방향, 역방향) 조회 가능
		ListIterator<Integer> li2 = intlist.listIterator();
		while(li2.hasNext()) {
			System.out.println(li2.next());
		}

		//역방향
		while(li2.hasPrevious()) {
			System.out.println("[" + li2.previous() + "]");
		}
	}
}
```
