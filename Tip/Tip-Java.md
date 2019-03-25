# Tip - Java

## apple-banana-melon 와 같이 문자사이에 특정문자를 입력하고 싶을 때
- org.apache.commons.lang3.StringUtils 사용
```java
import static org.hamcrest.CoreMatchers.is;
import static org.junit.Assert.assertThat;
import java.util.ArrayList;
import java.util.List;
import org.apache.commons.lang3.StringUtils;
import org.junit.Test;

public class StringUtilsJoinExample {
     @Test
     public void testStringUtilsJoin() {
          List<String> list = new ArrayList<>();
          list.add("apple");
          list.add("banana");
          list.add("melon");
          String result = StringUtils.join(list, "-");
          assertThat(result, is("apple-banana-melon"));
     }
}
```

- java.util.StringJoiner 사용  (JDK 1.8 이상)
```java
import static org.hamcrest.CoreMatchers.is;
import static org.junit.Assert.assertThat;
import java.util.ArrayList;
import java.util.List;
import java.util.StringJoiner;
import org.junit.Test;

public class StringJoinerExample {
    @Test
    public void testStringJoiner() {
        List<String> list = new ArrayList<>();
        list.add("apple");
        list.add("banana");
        list.add("melon");
        StringJoiner strJoiner = new StringJoiner("-");
        
        for (String str : list) {
            strJoiner.add(str);
        }
        
        assertThat(strJoiner.toString(), is("apple-banana-melon"));
    }
}
```

- Collectors.joining 사용 (JDK 1.8 이상)
```java
import static org.hamcrest.CoreMatchers.is;
import static org.junit.Assert.assertThat;
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;
import org.junit.Test;

public class CollectorsJoiningExample {
     @Test
     public void testCollectorsJoining() {
          List<String> list = Arrays.asList("apple",  "banana", "melon");
          String result =  list.stream().collect(Collectors.joining("-"));
          assertThat(result, is("apple-banana-melon"));
     }
}
```

- 참고
  - 자바의신 2권
  - https://java.ihoney.pe.kr/503