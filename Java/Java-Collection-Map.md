# Collection - Map
**Map 인터페이스 구현한 클래스**<br>
Map > (키, 값) 쌍구조 배열<br>
ex) 지역번호: 02, 서울<br>
- key: 중복(x)
- value: 중복(o)

**Generic 지원**
- HashTable(구버전): 동기화 보장
- HashMap(신버전): 동기화 강제 하지 않음 (필요하면 사용 가능)

현재 의미 없다 (동기화) >> 현재 단일 Process에 단일 Thread >> stack 하나만 가지고 >> Sequential

## 예제
```java
import java.util.Collection;
import java.util.HashMap;
import java.util.Iterator;
import java.util.List;
import java.util.Set;

public class Ex12_Map_HashMap {
	public static void main(String[] args) {
		HashMap map = new HashMap();
		map.put("Tiger", "1004");
		map.put("scott", "1004");
		map.put("superman", "1007");

		//Collection 자료구조 (데이터 가질 수 있다) >> 프로그램 실행되는 동안
		//>> 메모리(휘발성) >> 프로그램 종료 >> 데이터 보존 (파일기반) 도서관.txt (구조, 관리)
		//>> RDBMS (관계형 DB) 엑셀시트
		//활용: 회원ID, 회원PW

		System.out.println(map.containsKey("tiger")); //key 값은 대소문자 구문
		System.out.println(map.containsKey("scott"));
		System.out.println(map.containsValue("1004"));

		//(key, value)
		//key 값을 가지고 value 얻어서 사용하는 것이 목적
		System.out.println(map.get("Tiger")); //key 가지고 value 검색
		System.out.println(map.get("hong")); //key가 없으면 null값을 리턴

		//put
		map.put("Tiger", 1008); //가능 (key 같은 경우 put value: overwrite)
		System.out.println(map.get("Tiger"));

		Object returnvalue = map.remove("superman");
		System.out.println(returnvalue); //value값 return
		System.out.println("size: " + map.size());
		System.out.println(map.toString());

		//KeySet (key 들의 집합)
		Set set = map.keySet(); //중복(x), 순서(x)
		//Set 인터페이스 구현하고 있는 객체를 new 하고 그 곳에 key 담고 그 주소값을 return
		//출력
		Iterator it = set.iterator();
		while(it.hasNext()) {
			System.out.println(it.next());
		}

		//사용해서 출력
		//map.values();
		Collection vlist = map.values();
		System.out.println(vlist.toString());
	}
}
```

## Quiz
```
시스템에 로그인 하는 시나리오
ID(o), PWD(o) >> 회원 (환영)
ID(o), PWD(x) >> 실패 (비번 다시 입력)

ID(x), PWD(x) >> 실패 (다시 입력)
ID(x), PWD(o)

Scanner 사용해서 ID, PWD 입력받으세요
loginmap 통해서 검증 로직 처리
ID 또는 PWD 틀리면 다시 입력 요청
ID, PWD 다 맞으면 회원님 방문 환영합니다 (출력 프로그램 종료)
```
```java
import java.util.HashMap;
import java.util.Iterator;
import java.util.Scanner;
import java.util.Set;

public class Ex13_HashMap_Quiz {
	public static void main(String[] args) {
		HashMap loginmap = new HashMap();
		loginmap.put("kim", "kim1004");
		loginmap.put("scott", "tiger");
		loginmap.put("lee", "kim1004");

		Scanner sc = new Scanner(System.in);
		while(true) {
			System.out.println("ID , PWD 입력해 주세요");
			System.out.print("ID:");
			String id = sc.nextLine().trim().toLowerCase();

			System.out.print("PWD:");
			String pwd = sc.nextLine().trim();

			if(!loginmap.containsKey(id)) {
				System.out.println("ID 틀려요 재입력 하세요");
			}else {
				if(loginmap.get(id).equals(pwd)) { //loginmap.get(id) >> value return
					System.out.println("회원님 방가방가 ^^");
					break;
				}else {
					System.out.println("비번 확인 하세요");
				}
			}
		}
	}
}
```

## Generic
HashMap generic 사용<br>
HashMap<K, V><br>
HashMap<String, String><br>
HashMap<String, Integer><br>
HashMap<String, Emp>: value값으로 객체를 (클래스) 사용
```java
import java.util.HashMap;
import java.util.Iterator;
import java.util.Map;
import java.util.Set;

//IO, Network, Thread >> ArrayList<Emp>, HashMap<String, Emp> 활용
class Student {
	int kor = 100;
	int math = 80;
	int eng = 20;
	String name;
	Student(String name) {
		this.name = name;
	}
}

public class Ex14_HashMap_Generic {
	public static void main(String[] args) {
		HashMap<String, Student> students = new HashMap<>();
		students.put("hong", new Student("홍길동"));
		students.put("kim", new Student("김유신"));

		Student std = students.get("hong");
		System.out.println(std.name);
		System.out.println(std.eng);

		//예외적으로
		//Map 안에 있는 key, value 모두 출력
		Set set = students.entrySet();
		Iterator it = set.iterator();
		while(it.hasNext()) {
			System.out.println(it.next());
		}

		//예외적인 CASE: value가 객체(object) 일 때
		//Map.Entry m 리턴 받으면 (타입)
		//m.geyKey(), m.getValue()
		for(Map.Entry m : students.entrySet()) {
			System.out.println(m.getKey() + "/" + ((Student)m.getValue()).name);
			// casting 통해서 테이터접근
		}

		/*
		일반적인 사례
		HashMap map = new HashMap();
		map.put("Tiger", "1004");
		map.put("scott", "1004");
		map.put("superman", "1007");

		Set set2 = map.entrySet();
		Iterator it = set2.iterator();
		while(it.hasNext()) {
			System.out.println(it.next());
		}
		*/
	}
}
```
