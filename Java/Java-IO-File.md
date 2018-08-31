# IO - File
File 클래스
- 파일(a.txt) 생성, 수정, 삭제
- 디렉토리(폴더) 생성, 삭제
- 주로 사용: 파일정보, 폴더정보
- 절대경로: C:\\, D:\\, C:\\Temp\\a.txt
- 상대경로: 나를 중심으로

참고
- C#: File, Directory 클래스

## #1
```java
import java.io.File;

public class Ex07_File_Directory {
	public static void main(String[] args) {
		String path="C:\\Temp\\file.txt"; //기존에 만들어진 파일 정보만
		File f = new File(path);
		//f.createNewFile() //파일생성
		String filename = f.getName();
		System.out.println("파일명" + filename);

		System.out.println("전체경로: " + f.getPath());
		System.out.println("절대경로: " + f.getAbsolutePath());
		System.out.println("폴더 여부: " + f.isDirectory());
		System.out.println("파일 여부: " + f.isFile());
		System.out.println("파일 크기: " + f.length() + "byte");
		System.out.println("부모경로: " + f.getParent());
		System.out.println("파일 존재 여부: " + f.exists());

		//f.delete()   return type >> true, false
		//f.canRead() 사용가능 여부(읽기전용인지 아닌지)
	}
}
```

## #2
```java
import java.io.File;

public class Ex08_File_Directory {
	//main 프로그램의 시작점
	public static void main(String[] args) {
		//System.out.println(args.length);
		//System.out.println(args[0]);
		//System.out.println(args[1]);

		if(args.length != 1) {
			System.out.println("사용법: java 파일명 [디렉토리명]");
			System.exit(0); //프로세스 강제종료
		}
		//java Ex08... C:\\Temp
		//args[0] = C:\\Temp 정보
		File f = new File(args[0]);
		if(!f.exists() || !f.isDirectory()) {
			//존재하지 않거나 또는 디렉토리가 아니라면
			System.out.println("유효하지 않은 경로");
			System.exit(0);
		}
		//존재하는 경로
		File[] files = f.listFiles(); //하위 자원 (각각을 파일 객체 타입으로)
		for(int i = 0; i < files.length; i++) {
			String name = files[i].getName(); //폴더명, 파일명
			System.out.println(files[i].isDirectory() ? "DIR: " + name : name);
		}
	}
}
```

## Format
Format 클래스

- 화폐단위 처리
```java
DecimalFormat df = new DecimalFormat("#,###.0");
String result = df.format(1234567.89);
System.out.println(result);
```

- 날짜 형식 처리
```java
SimpleDateFormat sdf = new SimpleDateFormat("yyyy년 MM월 dd일");
String strDate = sdf.format(new Date());
System.out.println(strDate);
```

- 문자열 처리
```java
String message = "회원 Id :{0} \n회원 이름 : {1} \n회원 전화번호 : {2}";
String result = MessageFormat.format(message, userId, userName, userTel);
```

```java
import java.io.File;
import java.text.DecimalFormat;
import java.text.SimpleDateFormat;
import java.util.Date;

public class Ex09_File_Format {
	public static void main(String[] args) {
		File dir = new File("C:\\Temp");
		File[] files = dir.listFiles(); //모든 자원(폴더 또는 파일)

		for(int i = 0; i < files.length; i++) {
			File file = files[i];
			String name = file.getName(); //폴더명, 파일명

			SimpleDateFormat df = new SimpleDateFormat("yyyy-MM-dd HH:mma");
			String atrribute = "";
			String size = "";

			if(files[i].isDirectory()) {
				atrribute = "<DIR>";
			}else { //나머지는 파일(a.txt, copy.jpg ...)
				size = file.length() + "byte";
				atrribute = file.canRead() ? "R" : "";
				atrribute += file.canWrite() ? "W" : "";
				atrribute += file.isHidden() ? "H" : "";
			}
			System.out.printf("%s   %3s   %10s   %s   \n",
							df.format(file.lastModified()),
							atrribute,
							size,
							name);
		}
	}
}
```
