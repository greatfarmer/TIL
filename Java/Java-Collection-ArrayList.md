# Collection - ArrayList

## # 1
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

## # 2
### Emp.java
```java
package kr.or.bit;

public class Emp {
	private int empno;
	private String ename;
	private String job;

	public Emp(int empno, String ename, String job) {
		this.empno = empno;
		this.ename = ename;
		this.job = job;
	}

	public int getEmpno() {
		return empno;
	}

	public void setEmpno(int empno) {
		this.empno = empno;
	}

	public String getEname() {
		return ename;
	}

	public void setEname(String ename) {
		this.ename = ename;
	}

	public String getJob() {
		return job;
	}

	public void setJob(String job) {
		this.job = job;
	}

	@Override
	public String toString() {
		return "Emp [empno=" + empno + ", ename=" + ename + ", job=" + job + "]";
	}
}
```

### Ex03_ArrayList_Object_KeyPoint.java
```java
import java.util.ArrayList;

import kr.or.bit.Emp;

public class Ex03_ArrayList_Object_KeyPoint {
	public static void main(String[] args) {
		//정적배열(Array)
		//사원 1명을 만드세요
		Emp e = new Emp(100, "김유신", "군인");
		System.out.println(e.toString());

		//정적배열(Array) 사용
		//사원 2명 만드세요
		Emp[] emplist = {new Emp(10, "김씨", "IT"), new Emp(20, "박씨", "SALES")};

		for(Emp em : emplist) {
			System.out.println(em.toString());
		}

		//ArrayList를 사용해서 사원 2명을 만드세요
		ArrayList elist = new ArrayList();

		elist.add(new Emp(1,"김","IT"));
		elist.add(new Emp(2,"박","영업"));

		System.out.println(elist.toString());

		//for문 사용해서 사원 데이터 정보 출력
		for(int i = 0; i < elist.size(); i++) {
			//System.out.println(elist.get(i).toString());
			//System.out.println(((Emp)elist.get(i)).toString());
			Emp m = (Emp)elist.get(i);
			System.out.println(m.getEmpno() + " / " + m.getEname() + " / " + m.getJob());
		}

		//개선된 for문
		for(Object obj : elist) {
			Emp m = (Emp)obj;
			System.out.println(m.getEmpno());
		}

		//Object 불편 -> 실제 사용은 제너릭(generic)
		//generic
		ArrayList<Emp> elist2 = new ArrayList<Emp>();
		elist2.add(new Emp(1, "A", "IT"));
		elist2.add(new Emp(2, "B", "SALES"));

		for(Emp em : elist2) {
			System.out.println(em.getEmpno() + "/" + em.getEname() + "/" + em.getJob());
		}
	}
}
```
