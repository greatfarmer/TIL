# 문자열(String)

## #1 String Class
```java
//String 클래스
//이 수업을 듣고 String 클래스다 라는 사실을 지우세요
//String 사용방법이 8가지 기본타입과 동일
//String str = "홍길동";

//1. String 클래스: 데이터 저장 자료구조 >> char[]배열 사용
//2. String ename = "a길동" > [a][길][동]
//3. String s = new String("ABCD"); // 원칙

public class Ex05_String_Class {
	public static void main(String[] args) {
		String str = "ABCD";
		System.out.println(str.length()); //String 데이터 char[] 배열
		System.out.println(str); //str.toString() 결과

		String str1 = "AAA";
		String str2 = "AAA";
		System.out.println("str1: " + str1.toString());
		System.out.println("str2: " + str2.toString());
		System.out.println(str1 == str2); //false
		//String 비교 (주소안에 있는 값을 비교)
		//**Point (문자열의 비교는: equals)
		System.out.println(str1.equals(str2)); //AAA, AAA 같니

		String str3 = new String("BBB"); //new 무조건 다른 메모리에 생성
		String str4 = new String("BBB");
		System.out.println(str3 == str4); //주소값 비교
		System.out.println(str3 + " / " + str4);
		System.out.println("반드시(무조건): " + str3.equals(str4));
		//문자 비교는 반드시! 무조건! equals() 사용
	}
}
```

## #2 String Function
```java
import java.util.StringTokenizer;

//String 클래스 (다양한 함수)
//개발에서 많이 사용 (데이터 대부분 문자열 데이터: 조합처리)
//String 클래스는 static 함수도 제공한다 (public)
//String 클래스 new를 통해서 사용가능한 일반함수도 가지고 있다 (public)
public class Ex06_String_Function {
	public static void main(String[] args) {
		String str = ""; //문자열의 초기화

		String[] strarr = {"aaa", "bbb", "ccc"};
		for(String s : strarr) {
			System.out.println(s);
		}

		//String 클래스 함수
		String ss = "hello";
		String concatstr = ss.concat(" world");
		System.out.println(concatstr); //hello world

		//boolean bo = ss.contains("el");
		//boolean bo = ss.contains("e");
		boolean bo = ss.contains("elo");
		System.out.println(bo); //false

		String ss2 = "a b c d"; //[a][ ][b][ ][c][ ][d]
		System.out.println(ss2.length());
		String filename = "hello java, world";
		System.out.println(filename.indexOf(",")); //시작 위치(index)
		System.out.println(filename.indexOf("java"));
		//활용 (내가 원하는 단어가 당신이 제시한 문장에 포함되어 있다면
		System.out.println(filename.lastIndexOf("a"));
		System.out.println(filename.lastIndexOf("javb")); //배열에서 -1 없다는 표현
		//return -1 (값이 없다)

		//length(), indexof(), substring(), replace(), split()...
		String result = "superman";
		System.out.println(result.substring(0));
		System.out.println(result.substring(2));
		System.out.println(result.substring(0,  0));
		System.out.println(result.substring(0,  1));
		System.out.println(result.substring(1,  1)); //index endIndex - 1
		System.out.println(result.substring(2,  3));

		/*
		Returns a string that is a substring of this string.
		The substring begins at the specified beginIndex and
		extends to the character at index endIndex - 1.
		Thus the length of the substring is endIndex-beginIndex.
		 */

		//Quiz
		String filename2 = "bit.png";
		//aaaaa.hwp, bbbbbb.mpeg 일수도 있다
		//여기서 파일명과 확장명을 분리해서 출력하시오.

		//ex)

		int dot = filename2.indexOf(".");
		String front = filename2.substring(0, dot);
		String end = filename2.substring(dot+1, filename2.length());
		String end2 = filename2.substring(++dot);

		System.out.println("원본: " + filename2);
		System.out.println("파일명: " + front + " / 확장명: " + end);
		System.out.println("파일명: " + front + " / 확장명: " + end2);

		//replace
		String s4 = "ABCD";
		String result4 = s4.replace("A", "안녕");
		System.out.println(result4);

		System.out.println(s4.charAt(0));
		System.out.println(s4.charAt(3));
		System.out.println(s4.endsWith("CD"));
		System.out.println(s4.equalsIgnoreCase("aBcD"));

		//POINT: split
		String s6 = "슈퍼맨,팬티,노랑색,우하하,우하하";
		String[] namearry = s6.split(",");
		for(String s : namearry) {
			System.out.println(s);
		}
		String filename3 = "bit.hwp";
		String[] arry = filename3.split("\\."); //정규표현식을 문자처럼 인식 \
		System.out.println(arry.length);
		for(String s : arry) {
			System.out.println(s);
		}
		//정규표현식
		//Java, JavaScript, DB .....
		//010-{\d4}-0000
		//010-000-0000 문자가 정규표현 형식에 일치하는가 (false)

		String s7 = "a/b,c.d-f";
		StringTokenizer sto = new StringTokenizer(s7, "/,.-");
		while(sto.hasMoreTokens()) {
			System.out.println(sto.nextToken());
		}

		//넌센스 quiz
		String s8 = "홍        길                 동";
		//저장 > "홍길동" 공백제거 저장
		System.out.println(s8.replace(" ", ""));

		String s9 = "             홍길동                        ";
		System.out.println(">" + s9 + "<");
		System.out.println(">" + s9.trim() + "<");

		String s10 = "     홍        길         동            ";
		//System.out.println(">" + s10.replace(" ", "") + "<");
		String re = s10.trim();
		String re2 = re.replace(" ", "");
		System.out.println(re2); //무식해요
		//여러개의 함수를 적용할때는
		//method chain 기법
		System.out.println(s10.trim().replace(" ", ""));

		//Quiz-1
		//String snumstr = "";
		int sum = 0;
		String[] numarr = {"1", "2", "3", "4", "5"};
		//문제: 배열에 있는 문자값들의 합을 sum 변수에 담아서 출력하세요
		for(String s : numarr) {
			sum += Integer.parseInt(s);
		}
		System.out.println("합: " + sum);

		//Quiz-2
		String jumin = "123456-1234567";
		//문제: 주민번호의 합을 구하세요
		 int sum2 = 0;
		 for(int i = 0 ; i < jumin.length() ; i++){
				 String numstr = jumin.substring(i,i+1);
				 if(numstr.equals("-"))continue;
				 sum2+= Integer.parseInt(numstr);
			 }
		  System.out.println("주민번호합 :  " + sum2);

	    //주민번호의 합을 구하세요 _2
 	    //String jumin2 = jumin.replace("-", "").split(regex);
		   String[] numarr2 = jumin.replace("-", "").split("");
		   int sum3 =0;
		   for(String i : numarr2){
			   sum3 += Integer.parseInt(i);
		   }
		   System.out.println("주민번호합2 : " + sum3);

	   //주민번호의 합을 구하세요 _3
		   String jumin4 = jumin.replace("-", "");
		   int sum4 = 0;
		   for(int i = 0; i < jumin4.length() ; i++){
			   sum4+=Integer.parseInt(String.valueOf(jumin4.charAt(i)));
		   }
		   System.out.println("주민번호합3 : " + sum4);

		   //format (web) String 클래스 static 함수 > format
		   //prinf() cmd 모드
		   String str5 = String.format("%d-%s",10,"AA");
		   System.out.println(str5);
		   //static : valueof , format
	}
}
```

## #3 Quiz
```java
import java.util.Scanner;
//주민번호 : 뒷번호 첫자리 : 1,3 남자 , 2,4 여자 라고 출력 ...
//main 함수 Scanner  사용 주민번호 입력받고
//앞:6자리 뒷:7자리
//입력값 : 123456-1234567
//1. 자리수 체크 (기능)함수 (14 ok)
//2. 뒷번호 첫번째 자리값 1~4까지의 값만 허용 기능함수
//3. 뒷번호 첫번째 자리값을 가지고 1,3 남자 , 2,4 여자 출력 기능함수
//3개의 함수 static
public class Ex07_String_Total_Quiz {

	static boolean juminCheck(String str) {
		return str.length() == 14 ? true : false;
	}

	static boolean JunminFirstNumber(String str) {
		boolean numcheck = false;
		int num = Integer.parseInt(str.substring(7, 8));
		if (num > 0 && num < 5) {
			numcheck = true;
		}
		return numcheck;
	}

	static void JuminDisplay(String ssn) {
		// CASE1 >
		// String gender= ssn.replace("-","").substring(6,7);
		// int numgender = Integer.parseInt(gender);
		// if(numgender%2 == 0)System.out.println("여자");
		// if(numgender%2 == 1)System.out.println("남자");

		// CASE2 >
		char cgen = ssn.replace("-", "").charAt(6);
		// 123456-1234567 -> 1234561234567 > 123456[1]234567 추출> '1'
		switch (cgen) {
			case '1': // break 생략
			case '3':
				System.out.println("남자^^");
				break;
			case '2': // break 생략
			case '4':
				System.out.println("여자^^");
				break;
			default:
				System.out.println("중성");
		}
	}
	public static void main(String[] args) {

		String ssn = "";
		do {
			Scanner sc = new Scanner(System.in);
			System.out.print("주민번호 입력:");
			ssn = sc.nextLine();
		} while (!juminCheck(ssn) || !JunminFirstNumber(ssn));

		// 둘다 true 인경우  > false || false 탈출
		// !true || !true => false || false 탈출
		JuminDisplay(ssn);
	}
}
```
