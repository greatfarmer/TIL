# 10809: 알파벳 찾기
> https://www.acmicpc.net/problem/10809

## 문제
알파벳 소문자로만 이루어진 단어 S가 주어진다.
각각의 알파벳에 대해서, 단어에 포함되어 있는 경우에는 처음 등장하는 위치를, 포함되어 있지 않은 경우에는 -1을 출력하는 프로그램을 작성하시오.

## 입력
첫째 줄에 단어 S가 주어진다. 단어의 길이는 100을 넘지 않으며, 알파벳 소문자로만 이루어져 있다.

## 출력
각각의 알파벳에 대해서, a가 처음 등장하는 위치, b가 처음 등장하는 위치, ... z가 처음 등장하는 위치를 공백으로 구분해서 출력한다.

만약, 어떤 알파벳이 단어에 포함되어 있지 않다면 -1을 출력한다. 단어의 첫 번째 글자는 0번째 위치이고, 두 번째 글자는 1번째 위치이다.

## 예제 입력
baekjoon

## 예제 출력
1 0 -1 -1 2 -1 -1 -1 -1 4 3 -1 -1 7 5 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1

## Java (2018-10-03)
```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Collection;
import java.util.HashMap;
import java.util.Map;

class Alphabet {
	public Map<Integer, Integer> getAlphabetMap() {
		// 초기 알파벳 소문자 map값 생성
		Map<Integer, Integer> map = new HashMap<>();
		for (int i = 97; i <= 122; i++) {
			map.put(i, -1);
		}

		return map;
	}

	public void getCharLocation(String s) {
		Map<Integer, Integer> resultMap = getAlphabetMap();

		for (int i = 0; i < s.length(); i++) {
			int asciiNum = (int)s.charAt(i);

			if (resultMap.get(asciiNum) != -1) continue; // 기존에 값이 있을 경우 continue
			resultMap.replace(asciiNum, i);
		}

		// map의 value 출력
		Collection resultValues = resultMap.values();
		String result = resultValues.toString().replace("[", "").replace(",", "").replace("]", "");
		System.out.println(result);
	}
}

public class P10809_SearchAlphabet {
	public static void main(String[] args) throws IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		String s = br.readLine();

		if (s.length() == 0 || s.length() > 100) return; // 단어의 길이는 100 이하
		Alphabet alphabet = new Alphabet();
		alphabet.getCharLocation(s);
	}
}
```
