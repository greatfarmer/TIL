# 정규표현식(Regular Expression)

> https://ko.wikipedia.org/wiki/정규_표현식

정규 표현식(正規表現式, 영어: regular expression, 간단히 regexp 또는 regex, rational expression) 또는 정규식(正規式)은 특정한 규칙을 가진 문자열의 집합을 표현하는 데 사용하는 형식 언어이다. 정규 표현식은 많은 텍스트 편집기와 프로그래밍 언어에서 문자열의 검색과 치환을 위해 지원하고 있으며, 특히 펄과 Tcl은 언어 자체에 강력한 정규 표현식을 구현하고 있다.

컴퓨터 과학의 정규 언어로부터 유래하였으나 구현체에 따라서 정규 언어보다 더 넓은 언어를 표현할 수 있는 경우도 있으며, 심지어 정규 표현식 자체의 문법도 여러 가지 존재하고 있다. 현재 많은 프로그래밍 언어, 텍스트 처리 프로그램, 고급 텍스트 편집기 등이 정규 표현식 기능을 제공한다. 일부는 펄, 자바스크립트, 루비, Tcl처럼 문법에 내장되어 있는 반면 닷넷 언어, 자바, 파이썬, POSIX C, C++ (C++11 이후)에서는 표준 라이브러리를 통해 제공한다. 그 밖의 대부분의 언어들은 별도의 라이브러리를 통해 정규 표현식을 제공한다.

정규 표현식은 검색 엔진, 워드 프로세서와 문서 편집기의 찾아 바꾸기 대화상자, 또 sed, AWK와 같은 문자 처리 유틸리티, 어휘 분석에 사용된다.

## 기초 강의 by 프로그래머스
> [정규표현식 | 프로그래머스](https://programmers.co.kr/learn/courses/11)

### 공통

- `\d` 숫자(한글자 씩)
- `\w` 글자(문자 또는 숫자)
  - 특수문자 포함하지 않음
  - `_`(언더스코어) 포함
- `\d+` 하나 혹은 그 이상 연결된 숫자
- `\d*` 숫자가 0개 이상
- `[1-9]\d*` 자연수
- `?` 있거나 없거나
- `[- ]` - 또는 (공백)이 있거나 없다는 조건
- `\d{2}` 정확히 2번의 숫자가 나타나는걸 의미
- `\d{2,3}` 숫자가 2~3번 나오는 것을 의미
	- `{n,m} Qunatifier`에서, `{n,m}`은 `{n, m}`과 같이 사용할 수 없다
	- 콤마 주위에 공백을 사용하지 않도록 주의
- `[aeiou]` 소문자 모음(a,e,i,o,u)만 골라서 보고 싶을 때
- `[a-z]` 영어 소문자 a부터 z사이에 있는 글자를 모두 선택
- `[a-z]+` 연속된 영어 소문자
- `[가-힣]+` 연속된 모든 한글
	- `ㄱㄴㄷ`이나 `ㅏㅑㅓㅕ` 같은 낱글자는 포함되지 않음
- `\s` 공백문자(스페이스, 탭, 뉴라인)
- `\S` 공백문자를 제외한 문자
- `\D` 숫자를 제외한 문자
- `\W` 글자 대표문자를 제외한 글자들(특수문자, 공백 등)

### 언어별 정규표현식 사용

#### java
- 자바에서는 `\`를 사용하기 위해서는 `\\`를 적어주어야 함

```java
import java.io.Console;
import java.util.regex.Pattern;
import java.util.regex.Matcher;

public class MyRegex{
	public static void main(String[] args){
		String searchTarget = "Luke Skywarker 02-123-4567 luke@daum.net\n다스베이더 070-9999-9999 darth_vader@gmail.com\nprincess leia 010 2454 3457 leia@gmail.com";

		Pattern pattern = Pattern.compile("\\d+"); //여기에 정규표현식을 적습니다.
		Matcher matcher = pattern.matcher(searchTarget);
		while(matcher.find()){
			System.out.println(matcher.group(0));
		}
	}
}
```

#### javascript
```js
var searchTarget = "Luke Skywarker 02-123-4567 luke@daum.net\n다스베이더 070-9999-9999 darth_vader@gmail.com\nprincess leia 010 2454 3457 leia@gmail.com";

/* 아래 코드의 /와 /g가운데에 정규표현식을 넣으세요.
 * g는 global의 약자로, 정규표현식과 일치하는 모든 내용을 찾아오라는 옵션입니다.
 * g가 있을 때와 없을 때 출력이 차이나는걸 확인 해 보세요.
 */
var regex = /\d+/g;
//var regex = /여기에 정규표현식을 입력하세요/g
console.log(searchTarget.match(regex));
```

#### `c#`
- c#에서는 `\`를 사용하기 위해서는 `\\`를 적어주어야 함

```cs
using System;
using System.Text.RegularExpressions;

public class RegexTest {
	public static void Main() {
		string regex = "\\d+"; //여기에 정규표현식을 적습니다.
		string searchTarget = "Luke Skywarker 02-123-4567 luke@daum.net\n다스베이더 070-9999-9999 darth_vader@gmail.com\nprincess leia 010 2454 3457 leia@gmail.com";

		foreach (Match m in Regex.Matches(searchTarget, regex)){
			Console.WriteLine(m.Value);
		}
	}
}
```

## 참고 사이트
- [Online regex tester, debugger with highlighting for PHP, PCRE, Python, Golang and JavaScript.](https://regex101.com/)
- [정규표현식을 소개합니다](http://www.nextree.co.kr/p4327/)
