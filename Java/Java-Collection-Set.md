# Collection - Set

Set 인터페이스 구현하는 클래스<br>
Set 순서(x), 중복(x) 이런 데이터 집합을 다룰 때<br>
순서 (넣은 순서를 보장하지 않음) <br>

- HashSet
- TreeSet

## HashSet

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

## TreeSet

```java
import java.util.HashSet;
import java.util.Iterator;
import java.util.LinkedHashSet;
import java.util.Set;
import java.util.TreeSet;

public class Ex11_Set_TreeSet {
	public static void main(String[] args) {
		//순서유지(x), 중복(x)
		HashSet<String> hs = new HashSet<>();
		hs.add("B");
		hs.add("A");
		hs.add("F");
		hs.add("K");
		hs.add("G");
		hs.add("D");
		hs.add("P");
		hs.add("A");
		System.out.println(hs.toString());

		//HashSet 확장 > LinkedHashSet (순서유지: 주소값으로) : Linked(객체 주소) >> node
		Set<String> hs2 = new LinkedHashSet<>();
		hs2.add("B");
		hs2.add("A");
		hs2.add("F");
		hs2.add("K");
		hs2.add("G");
		hs2.add("D");
		hs2.add("P");
		hs2.add("A");
		//Array 아님 (주소값 연결)
		System.out.println(hs2.toString());

		//자료구조 (순서(x), 중복(x), 정렬(o))
		//검색하거나 정렬을 필요로 하는 자료구조 (알고리즘)
		//TreeSet
		//데이터 트리(이진 트리): 정렬되고 많은 양의 데이터 저장 효율적
		//검색 속도 빠름

		//TreeSet를 사용해서 로또 구현
		//1~45 난수 >> 6개 >> 중복값(x) >> 정렬(o)
		//결과 출력 (Iterator)

		Set<Integer> ls = new TreeSet<>();

		while(ls.size() < 6) {
			int num = (int)(Math.random()*45 + 1);
			ls.add(num);
		}

		int sum = 0;
		Iterator<Integer> it = ls.iterator();
		while(it.hasNext()) {
			//System.out.println(it.next());
			sum+=it.next();
		}
		System.out.println(ls.toString());
		System.out.println(sum);
	}
}
```
