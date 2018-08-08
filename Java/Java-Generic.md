# 제네릭(Generic)
JDK 1.5부터 지원
1. Object 타입 저항(탈피)
2. 타입 안정성(타입 강제성)
3. 형 변환(casting) 문제 해결

```java
import java.util.ArrayList;

class Person {
	int age = 100;
}

//Generic 클래스 설계 >> 타입을 강제해서 객체를 생성할 수 있다
//MyGen<String> m = new MyGen<String>();
class MyGen<T> { //Type Parameter
	T obj;
	void add(T obj) {
		this.obj = obj;
	}
	T get() {
		return this.obj;
	}
}

public class Ex06_Generic {
	public static void main(String[] args) {
		MyGen<String> my = new MyGen<String>();
		my.add("Hello");
		String result = my.get();
		System.out.println(result);

		//Person 타입을 강제하는
		//MyGen 타입의 객체 생성
		MyGen<Person> myobj = new MyGen<Person>();
		myobj.add(new Person());
		Person p = myobj.get();
		System.out.println(p.age);

		//Quiz (Generic x)
		ArrayList list = new ArrayList();
		list.add(500);
		list.add(new Person());
		list.add("홍길동");

		//개선된 for문을 통해 출력 >> 500, 100, "홍길동"
		for(Object obj : list) {
			if(obj instanceof Person) {
				Person person = (Person)obj;
				System.out.println(person.age);
			}else {
				System.out.println(obj);
			}
		}

		//위 처럼 사용하지 말자 -> generic 사용 배경
		ArrayList<Person> alist = new ArrayList<>();
		alist.add(new Person());
		System.out.println(alist.get(0).age);
	}
}
```
