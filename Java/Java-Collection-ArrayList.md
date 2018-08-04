# Collection - ArrayList

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Collections;
import java.util.List;
import java.util.Vector;

//ArrayList 통해서 객체 다루기

public class Ex02_ArrayList {
	public static void main(String[] args) {
		ArrayList arraylist = new ArrayList();
		//Tip: capacity() 함수는 Vector에는 존재하지만, ArrayList에는 존재하지 않는다.
		//대신 ArrayList에서 capacity처럼 초기값을 정해줄 수 있다.
		//ArrayList arraylist2 = new ArrayList(10);

		arraylist.add(100);
		arraylist.add(200);
		arraylist.add(300);

		System.out.println(arraylist.toString());
		for(int i = 0; i < arraylist.size(); i++) {
			System.out.println(arraylist.get(i));
		}
		System.out.println("현재[0]: " + arraylist.get(0));
		arraylist.add(0, 1111);

		System.out.println("현재[0]: " + arraylist.get(0));
		System.out.println(arraylist.toString());

		//데이터 삽입 (add): 중간에 >> 데이터 이동
		//중간(비순차적인) 데이터 추가, 삭제하는 작업은 성능상 단점
		//순차적인 데이터 추가, 삭제 장점

		//ArrayList 함수 활용
		System.out.println(arraylist.contains(200));
		System.out.println(arraylist.contains(333));

		System.out.println(arraylist.isEmpty()); // (true, false): true가 비어 있는 것
		arraylist.clear();
		System.out.println(arraylist.isEmpty()); //clear >> size = 0 >> true

		arraylist.add(101);
		arraylist.add(102);
		arraylist.add(103);
		System.out.println(arraylist.toString());

		//0번째 방에 있는 데이터 삭제
		Object value = arraylist.remove(0); //필요하다면 지워지는 값을 받을 수 있다
		System.out.println(value);
		System.out.println(arraylist.toString());

		ArrayList list = new ArrayList();

		list.add("가");
		list.add("나");
		list.add("다");
		list.add("가");

		System.out.println("ArrayList:순서유지: " + list.toString());
		list.remove("가"); //값을 주면 앞에서 찾아서 삭제
		System.out.println("ArrayList 삭제: " + list.toString());

		//List 인터페이스를 부모  타입으로
		List li = new ArrayList();
		li = new Vector();

		li.add("가");
		li.add("나");
		li.add("다");
		li.add("라");

		List li4 = li.subList(0, 2); // new ArrayList() >> add("가"), add("나")
		System.out.println(li.toString());
		System.out.println(li4.toString());

		ArrayList alist = new ArrayList();
		alist.add(50);
		alist.add(1);
		alist.add(7);
		alist.add(40);
		alist.add(7);
		alist.add(15);

		System.out.println("before: " + alist.toString());
		//Arrays.sort(); //보조클래스
		Collections.sort(alist);

		System.out.println("after: " + alist.toString());
	}
}
```
