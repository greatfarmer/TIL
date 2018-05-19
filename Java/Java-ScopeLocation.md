# Scope Location

```java

import kr.or.bit.common.CommonClassPublic;

/*
클래스 == 설계도 == 데이터 타입

클래스의 구성요소: **필드(고유, 상태, 참조) - 외우기, 생성자(필드의 초기화를 목적으로 하는 함수), 함수
접근자(한정자, Modifier): public, private, default, protected (protected는 상속관계에서 사용)

1. public
public class Test{}

2. default
class Test{} >> default 접근자가 생략되어 있다
(default 접근자는 폴더(같은 폴더에 위치: 같은 폴더 안에, 다른 폴더에 있느냐에 따라 자원 사용여부 판단)

3. default class 언제 사용하는 거지? -> 같은 폴더 내에서 다른 클래스의 참조 목적으로, 연습목적으로

4. 하나의 자바 파일은 여러개의 클래스를 가질 수 있다 (Test.java) => 참, True
	EX01_main.java 라는 파일은 public class Ex01_main 이라는 클래스를 가지고 있다.
 */

class TTT { // default class TTT (같은 폴더에 같은 클래스명은 존재 할 수 없다)
	int a; // default

	void print() { // default
		System.out.println(a);
	}
}

class PPP {

}

public class Ex01_main {
	public static void main(String[] args) {
		// public이라도 다른 프로젝트간의 참조는 되지 않는다. 참조하기위해서는 라이브러리에서 통째로 가져와야 한다.
		// 1. kr.or.bit.common 폴더 안에 있는 클래스를 사용하려면
		// a) Full name 사용
		//	kr.or.bit.common.CommonClass c = new kr.or.bit.common.CommonClass();
		// b) import kr.or.bit.common.CommonClass;
		// 단, 접근자가 public일 경우
		//	CommonClass c = new CommonClass();

		CommonClassPublic common = new CommonClassPublic();
		// common.a = 10; a라는 변수: public

		TTT t = new TTT();
		t.a = 100;
		t.print();
	}
}
```

```java
import kr.or.bit.common.TTT;

public class Ex02_main {
	public static void main(String[] args) {
		TTT t = new TTT();
		// kr.or.bit.common.TTT t2 = new kr.or.bit.common.TTT();
		TTT t2 = new TTT(); // **import 된 것의 클래스가 우선 사용되고 import 된 클래스만 인식된다.


	}
}
```
