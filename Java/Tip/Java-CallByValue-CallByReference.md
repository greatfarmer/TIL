# Call By Value vs Call By Reference

- Call By Value
```java
package reference;

public class CallByValue {
    public static void main(String[] args) {
        int a = 10;
        int b = a;

        b = 20;

        System.out.println(a); // 10
        System.out.println(b); // 20
    }
}
```

- Call By Reference
```java
package reference;

public class CallByReference {
    public static void main(String[] args) {
        Animal ref_a = new Animal();
        Animal ref_b = ref_a;

        ref_a.age = 10;
        ref_b.age = 20;

        System.out.println(ref_a.age); // 10
        System.out.println(ref_b.age); // 20
    }
}

class Animal {
    public int age;
}

```

> 출처: 스프링 입문을 위한 자바 객체 지향의 원리와 이해, 김종민, 위키북스