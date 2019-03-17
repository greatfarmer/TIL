# 1181: 단어 정렬
> https://www.acmicpc.net/problem/1181

## 문제
알파벳 소문자로 이루어진 N개의 단어가 들어오면 아래와 같은 조건에 따라 정렬하는 프로그램을 작성하시오.
1. 길이가 짧은 것부터
2. 길이가 같으면 사전 순으로

## 입력
첫째 줄에 단어의 개수 N이 주어진다. (1≤N≤20,000) 둘째 줄부터 N개의 줄에 걸쳐 알파벳 소문자로 이루어진 단어가 한 줄에 하나씩 주어진다. 주어지는 문자열의 길이는 50을 넘지 않는다.

## 출력
조건에 따라 정렬하여 단어들을 출력한다. 단, 같은 단어가 여러 번 입력된 경우에는 한 번씩만 출력한다.

## 예제 입력
```
13
but
i
wont
hesitate
no
more
no
more
it
cannot
wait
im
yours
```

## 예제 출력
```
i
im
it
no
but
more
wait
wont
yours
cannot
hesitate
```

## Java (2019-03-17)
```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.*;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int n = 0;
        while(true) {
            n = Integer.parseInt(br.readLine());
            if(n < 1 || n > 20000) {
                System.out.println("(1≤N≤20,000)");
            } else {
                break;
            }
        }

        Map<Integer, Set<String>> map = new HashMap<>();
        for (int i = 0; i < n; i++) {
            String word = "";
            while(true) {
                word = br.readLine();
                if (word.length() > 50) {
                    System.out.println("50자 이내로 입력해주세요");
                } else {
                    break;
                }
            }

            putWordByLength(map, word);
        }

        printList(getSortedWordList(map));
    }

    private static void putWordByLength(Map<Integer, Set<String>> map, String word) {
        int length = word.length();
        if (map.containsKey(length)) {
            map.get(length).add(word);
        } else {
            Set<String> set = new HashSet<>();
            set.add(word);
            map.put(length, set);
        }
    }

    private static List<String> getSortedWordList(Map<Integer, Set<String>> map) {
        List<String> sortedWordList = new ArrayList<>();
        TreeMap<Integer, Set<String>> treeMap = new TreeMap<>(map);

        Iterator<Integer> iteratorKey = treeMap.keySet().iterator();
        while(iteratorKey.hasNext()) {
            int key = iteratorKey.next();
            sortedWordList.addAll(sort(treeMap.get(key)));
        }

        return sortedWordList;
    }

    private static List<String> sort(Set<String> set) {
        List<String> list = new ArrayList<>(set);
        Collections.sort(list);
        return list;
    }

    private static void printList(List<String> list) {
        for (String str : list) {
            System.out.println(str);
        }
    }
}
```
