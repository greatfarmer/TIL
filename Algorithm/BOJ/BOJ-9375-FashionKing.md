# 9375: 패션왕 신해빈
> https://www.acmicpc.net/problem/9375

## 문제
해빈이는 패션에 매우 민감해서 한번 입었던 옷들의 조합을 절대 다시 입지 않는다. 예를 들어 오늘 해빈이가 안경, 코트, 상의, 신발을 입었다면, 다음날은 바지를 추가로 입거나 안경대신 렌즈를 착용하거나 해야한다. 해빈이가 가진 의상들이 주어졌을때 과연 해빈이는 알몸이 아닌 상태로 며칠동안 밖에 돌아다닐 수 있을까?

## 입력
첫째 줄에 테스트 케이스가 주어진다. 테스트 케이스는 최대 100이다.

- 각 테스트 케이스의 첫째 줄에는 해빈이가 가진 의상의 수 n(0 ≤ n ≤ 30)이 주어진다.
- 다음 n개에는 해빈이가 가진 의상의 이름과 의상의 종류가 공백으로 구분되어 주어진다. 같은 종류의 의상은 하나만 입을 수 있다.

모든 문자열은 1이상 20이하의 알파벳 소문자로 이루어져있으며 같은 이름을 가진 의상은 존재하지 않는다.

## 출력
각 테스트 케이스에 대해 해빈이가 알몸이 아닌 상태로 의상을 입을 수 있는 경우를 출력하시오.

## 예제 입력 1
```
2
3
hat headgear
sunglasses eyewear
turban headgear
3
mask face
sunglasses face
makeup face
```

## 예제 출력 1
```
5
3
```

## 힌트
```
첫 번째 테스트 케이스는 headgear에 해당하는 의상이 hat, turban이며 eyewear에 해당하는 의상이 sunglasses이므로   (hat), (turban), (sunglasses), (hat,sunglasses), (turban,sunglasses)로 총 5가지 이다.
```

## Java (2019-06-02)
```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.HashMap;
import java.util.HashSet;
import java.util.Iterator;
import java.util.Map;
import java.util.Set;

public class P9375 {

    static BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    static BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

    public static void main(String[] args) throws IOException {
        run(br, bw);
        bw.flush();
        br.close();
        bw.close();
    }

    public static void run(BufferedReader br, BufferedWriter bw) throws IOException {
        int testCaseNum = Integer.parseInt(br.readLine());
        for (int i = 0; i < testCaseNum; i++) {
            int n = Integer.parseInt(br.readLine());
            Map<String, Set<String>> map = new HashMap<>();
            for (int j = 0 ; j < n; j++) {
                String[] item = inputItems(br);
                putItem(map, item);
            }
            bw.write(getResult(map) + "\n");
        }
    }

    public static String[] inputItems(BufferedReader br) throws IOException {
        return br.readLine().split("\\s");
    }

    public static void putItem(Map<String, Set<String>> map, String[] item) {
        String name = item[0];
        String type = item[1];

        if (!map.containsKey(type)) {
            Set<String> set = new HashSet<>();
            set.add(name);
            map.put(type, set);
        } else {
            Set<String> set = map.get(type);
            set.add(name);
        }
    }

    public static int getResult(Map<String, Set<String>> map) {
        int result = 1;
        Set keySet = map.keySet();
        Iterator<String> it = keySet.iterator();
        while (it.hasNext()) {
            String key = it.next();
            Set<String> value = map.get(key);
            result *= (value.size() + 1);
        }

        return result - 1;
    }

}
```
