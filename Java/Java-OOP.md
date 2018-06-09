# 객체지향언어(OOP: Object-Oriendted Programming)

## 객체지향언어의 특징
* 상속
* 캡슐화
* 다형성

## 클래스와 객체
### 클래스 (class)
* 정의 - 클래스란 객체를 정의해 놓은 것
* 용도 - 클래스는 객체를 생성하는데 사용

#### 클래스의 또 다른 정의
* 변수 - 하나의 데이터를 저장할 수 있는 공간
* 배열 - 같은 타입의 여러 데이터를 저장할 수 있는 공간
* 구조체 - 타입에 관계없이 서로 관련된 데이터들을 저장할 수 있는 공간
* 클래스 - 데이터와 함수의 결합 (구조체 + 함수)
    * 사용자 정의 타입 (User-defined type)
    * 프로그래머가 직접 새로운 타입을 정의할 수 있음
    * 서로 관련된 값을 묶어서 하나의 타입으로 정의

#### 객체 (object) = 인스턴스(instance)
* 정의 - 실제로 존재하는 것. 사물 또는 개념
* 용도 - 객체의 속성과 기능에 따라 다름

|클래스|객체|
|:---:|:---:|
|아파트 설계도|아파트|
|제품 설계도|제품|
|TV 설계도|TV|
|붕어빵 기계|붕어빵|

#### 객체의 구성요소
* 객체는 속성과 기능으로 이루어져 있음
* 속성은 변수로, 기능은 메서드로 정의

### 변수 (variable)
* 클래스 변수 (class variable)
* 인스턴스 변수 (instance variable)
* 지역 변수 (local variable)

### 함수(메서드): 기능의 최소단위 (method)
* 작업을 수행하기 위한 명령문의 집합
* 어떤 값을 입력받아서 처리하고 그 결과를 반환 (입력받는 값과, 반환하는 값이 없을 수도 있음)


### 함수의 종류
* void 사용하면 (돌려주는 값이 없다): return value가 없다
* return type: [8가지 기본 타입] / String / 참조(사용자정의) / 배열 / Collection / Interface
    * return type 있으면 반드시 구문안에서 return 키워드를 명시
* parameter: [8가지 기본 타입] / String / 참조(사용자정의) / 배열 / Collection / Interface


1. void, parameter가 있다: void print(String str) { 실행코드 }
2. void, parameter가 없다: void print() { 실행코드 }
3. return type, parameter가 있다: int print(int data) { return 100 + data; }
4. return type, parameter가 없다: int print() { return 200; }

##### ** 함수는 반드시 호출(Call) 되어야만 실행된다: 누군가 그의 이름을 불러주어야...

### 제어자(modifier)
* 접근 제어자 - public, protected, default, private
* 그 외 - static, final, abstract, native, transient, synchronized, volatile, strictfp

* public (공유: 폴더(package) 구분없이 모든 자원 공유)
* private(개인: 클래스 내부에서 공유, 참조변수는 볼 수 없어요(사용불가) >> 객체입장에서 접근 불가

### 객체지향언어 특징(캡슐화, 은닉화)
* 클래스 내부 자원에 적용 >> member field (instance variable): private int age;
    * private 의미: 직접할당을 막고 ** [간접할당]을 통해서 자원 보호
    * 설계자의 의도 (원하는 값만 처리) > private int age > 1 ~ 200까지 정수만 넣겠다. > 별도의 함수를 통해 제어
    * private 변수는 별도의 기능(변수 값을 write 함수, 변수의 값을 read 함수)
      캡슐화된 member field에서 값을 write, read 기능을 가진 함수
      * -> setter(쓰기), getter(읽기) 함수 이름 부여

```
private int age = 100;
  -setter 함수만 사용 가능
  -getter 함수만 사용 가능
  -setter, getter 함수 모두 사용 가능
```

* 클래스 내부 자원에 적용 >> method: private void call() {}
    * 함수를 private로 하는 이유: 클래스내부에서 다른 함수를 도와주는 역할 하는 함수 (보조함수)

### 메서드 오버로딩(method overloading)
```
하나의 이름으로 여러가지 기능을 하는 함수
사용: println() 대표적인 함수
특징: 오버로딩은 성능 향상에 도움을 준다 (x) -> 상관없다
     why) 개발자가 편하게 사용하려고
                설계시 어떤 점을 생각하면: 함수의 활용이 많은 경우 (parameter 변경)
                ex)System.out.println() : static이면서 overloading

문법: 함수(메서드)의 이름은 같고 parameter의 개수와 타입을 달리하는 방법
1. 함수(메서드)의 이름은 같아야 한다
2. [parameter] 개수 또는 [type]은 달라야 한다
3. return type은 overloading 판단 기준 (x) -> return type이 같아야 한다
4. parameter 순서가 다름을 인정한다
```

### 생성자 함수(constructor)
```
1.함수 ("특수한 목적"을 가지는 함수)
2.특수한 목적 (member field 초기화)

3.일반함수와 다른점
  3.1 함수의 이름이 고정 (class 이름과 동일)
  3.2 return type(x), void(x) >> 사실은 모든 생성자 함수 (void)
  3.3 why void (default): 실행시점: 객체생성과 동시에 호출되는 함수: 생성된 값을 받을 녀석이 없음
  3.4 일반함수 (이름을 호출: print();), 생성자 함수는 new를 통해서 class가 객체로 만들어 질 때

4.목적: 생성되는 객체마다 다른 초기값을 부여할 때

5.함수는 overloading이 가능하다 (생성자 함수도 overloading 사용)

new Car(); 메모리에 올릴 때 함수를 호출하면서 올리겠다는 의미 (default constructor)

class 생성시 default 생성자는 생략 가능 >> 컴파일러가 만듦
```

## 관련 예제 코드
> [Ex01_Ref_Type.java](https://drive.google.com/file/d/18nMyX6HLSWDVJGfzEwzu6h0QFnXfO4Gs/view?usp=sharing)

> [Ex02_MethodCall.java](https://drive.google.com/file/d/1S63S7zXAtDv9mm3kNSFamtMQNKi338JO/view?usp=sharing)

> [Ex03_Modifier.java](https://drive.google.com/file/d/1idgqxP59BlmDa18yF0H6kxqdqgoy4rN-/view?usp=sharing)

> [Ex04_Variable_Scope.java](https://drive.google.com/file/d/1vHelKH4kAQz9kTggkG06W2rUpQAtbNJm/view?usp=sharing)

> [Ex05_Static_CardMake.java](https://drive.google.com/file/d/1TAHpLfc5kRaWye1-cSr6vcmzeAXIz0Yb/view?usp=sharing)

> [Ex06_Static_Airplane.java](https://drive.google.com/file/d/1WVhL1lphvMGn8koLgS-ZPuAGhXRzAlNo/view?usp=sharing)

> [Ex07_Static_Method.java](https://drive.google.com/file/d/1GhTXoWL5GOIHOxw2z9emEA8A2HB1I59I/view?usp=sharing)

> [Ex08_Static_Method.java](https://drive.google.com/file/d/18o9ZhtFWsPH4Pibe0irJx5Wg5EIWippH/view?usp=sharing)

> [Ex09_do_while_Static_Menu.java](https://drive.google.com/file/d/1GGJUzad0LGVZcIFRulMTgw25Oynbf5bK/view?usp=sharing)

> [Ex10_Method_Call.java](https://drive.google.com/file/d/1IUbrLd_f-AbQzverTLzgzm4ZZwmMwx5-/view?usp=sharing)
