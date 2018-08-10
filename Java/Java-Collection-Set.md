# Collection - Set
Set 인터페이스 구현하는 클래스<br>
Set 순서(x), 중복(x) 이런 데이터 집합을 다룰 때<br>
순서 (넣은 순서를 보장하지 않음) <br>
- HashSet
- TreeSet

```java
import java.util.ArrayList;
import java.util.Collection;
import java.util.Collections;
import java.util.HashSet;
import java.util.Iterator;
import java.util.List;
import java.util.Set;

public class Ex10_Set_Interface {
	public static void main(String[] args) {
		HashSet<Integer> hs = new HashSet<>();
		hs.add(1);
		hs.add(100);
		hs.add(55);
		System.out.println(hs.toString());

		//중복 데이터 처리(POINT)
		boolean bo = hs.add(1);
		System.out.println(bo);
		System.out.println(hs.toString());
		hs.add(2);
		System.out.println(hs.toString());

		HashSet<String> hs2 = new HashSet<>();
		hs2.add("b");
		hs2.add("A");
		hs2.add("F");
		hs2.add("c");
		hs2.add("z");
		System.out.println(hs2.toString());

		//1.중복 허락하지 않음
		String[] obj = {"A", "B", "A", "C", "D", "B", "A"};
		HashSet<String> hs3 = new HashSet<>();
		for(int i = 0; i < obj.length; i++) {
			hs3.add(obj[i]);
		}
		System.out.println(hs3.toString());

		//Quiz
		//HashSet 사용해서 1~45까지 난수 6개를 넣으세요
		//단, 중복값 허용하지 않음
		//(int)(Math.Random()*45)+1
		Set set = new HashSet();

		//for문 사용
		/*
		for(int i = 0; set.size() < 6; i++) {
			int num = (int)(Math.random()*45)+1;
			set.add(num);
		}
		System.out.println(set.toString());
		*/

		//while문 사용
		int index = 0;
		while(set.size() < 6) {
			++index;
			set.add((int)(Math.random()*45)+1);
		}
		System.out.println(index);
		System.out.println(set.toString());

		HashSet<String> set3 = new HashSet<>();
		set3.add("AA");
		set3.add("DD");
		set3.add("AAC");
		set3.add("FFFF");
		System.out.println(set3.toString());

		Iterator<String> s = set3.iterator();
		while(s.hasNext()) {
			System.out.println(s.next());
		} //순서 (add한 순서) 보장 하지 않는다 (배열이 아니므로)

		//Collections.sort(list); //List 인터페이스 구현한 객체는 여기에 올 수 있다.
		//Collections.reverse(List<T> li);

		//Set 인터페이스 구현 자원: sort 의미 없다
		//List list = new ArrayList(set3);
		List list = new ArrayList(set3);
		System.out.println("before 무작위: " + list.toString());
		Collections.sort(list);
		System.out.println("after 정렬: " + list.toString());
		Collections.reverse(list);
		System.out.println("after 역정렬: " + list.toString());
	}
}
```
