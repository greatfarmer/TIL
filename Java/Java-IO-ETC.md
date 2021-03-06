# IO - ETC

## PrintWriter
```java
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.PrintWriter;

//PrintWriter 클래스를 사용하면 [출력형식] 정의
//System.out.printf() //String.foramt()
public class Ex11_PrintWriter {
	public static void main(String[] args) {
		try{
			//출력전용 (파일 생성 기능)
			PrintWriter pw = new PrintWriter("C:\\Temp\\homework.txt");
			pw.println("**************************************");
			pw.println("*      HOMEWORK                      *");
			pw.println("**************************************");
			pw.printf("%3s  : %5d  %5d  %5d  %5.1f ", "아무개",10,30,50, (float)((10+30+50)/3));
			pw.println();
			pw.close(); //파일에 write close()

			//read
			FileReader fr = new FileReader("C:\\Temp\\homework.txt");
			BufferedReader br = new BufferedReader(fr);
			String s="";
			while((s = br.readLine()) != null){
				System.out.println(s);
			}

			br.close();
			fr.close();

		}catch(Exception e){
			System.out.println(e.getMessage());
		}finally {
			//System.out.println("finally");
		}
	}
}
```

## PrintWriter String Find
```java
import java.io.BufferedReader;
import java.io.File;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;
import java.io.PrintWriter;

public class Ex12_PrintWrite_String_Find {

	String baseDir = "C:\\Temp"; //검색할 디렉토리
	String word = "HELLO"; //검색할 단어
	String save = "result.txt"; //검색 결과가 저장된 파일명

	public Ex12_PrintWrite_String_Find() throws IOException{

		File dir = new File(baseDir); //C:\\Temp

		if(!dir.isDirectory()){
				System.out.println("baseDir :" + "is not directory or exist" );
				System.exit(0);
		}
		//PrintWriter p = new PrintWriter("")
		PrintWriter writer = new PrintWriter(new FileWriter(save));
		BufferedReader br = null;

		File[] files = dir.listFiles(); //C:\\Temp  하위 자원(파일 ,폴더)
		for(int i = 0 ; i < files.length ; i++){
			if(!files[i].isFile()){
				continue; //파일이 아닌 경우 skip
			}

			br = new BufferedReader(new FileReader(files[i]));

			//각 파일의 데이터를 라인단위를 읽어서
			String line = "";
			while((line = br.readLine()) != null){
				if(line.indexOf(word) != -1){
					writer.write(word  + "=" + files[i].getAbsolutePath() + "\n");
				}
			}
			writer.flush();
		}

		br.close();
		writer.close();
	}

	public static void main(String[] args) {
		try {
			Ex12_PrintWrite_String_Find StringFind = new Ex12_PrintWrite_String_Find();
		} catch (IOException e) {
			e.printStackTrace();
		}
	}
}
```

## DataOutPutStream
보조 스트림 <br>
Java API : 8기본 타입 (타입별로 read ,write) 클래스 <br>
DataOutPutStream , DataInputStream <br>
데이터 작업 ,채팅 ...
```java
import java.io.DataOutputStream;
import java.io.FileOutputStream;

public class Ex13_DataOutPutStream {
	public static void main(String[] args) {
		int[] score = {100,60,55,94,12};

		try {
				FileOutputStream fos = new FileOutputStream("score.txt");
				DataOutputStream dos = new DataOutputStream(fos);
				for(int i = 0 ; i < score.length ;i++) {
					dos.writeInt(score[i]); //정수값 write > read(DataInputStream)
					//dos.writeUTF(str)
				}
				dos.close();
				fos.close();
		}catch (Exception e) {
			// TODO handle exception
		}
	}
}
```

## DataInputStream
```java
import java.io.DataInputStream;
import java.io.FileInputStream;

public class Ex14_DataInputStream {
	public static void main(String[] args) {
		int sum=0;
		int score=0;
		FileInputStream fis=null;
		DataInputStream dis=null;
		try {
				fis =  new FileInputStream("score.txt");
				dis = new DataInputStream(fis);
				while(true) {
					score = dis.readInt();
					System.out.println("Score int 타입 :" + score);
					sum+=score;
				}
		}catch (Exception e) {
			System.out.println("예외발생 : " + e.getMessage());
			System.out.println("sum결과 : " + sum);
		}finally {
			try {
				dis.close();
				fis.close();
			}catch (Exception e) {
				// TODO handle exception
			}
		}
	}
}
```

## ObjectOutputStream, ObjectInputStream
### UserInfo.java
```java
package kr.or.bit;

import java.io.Serializable;

//객체 통신
//객체를 네트워크, 파일간에, 프로세스간에 통신
//직렬화 (객체를 분해해서 줄을 세워 보내는 과정)
//역직렬화 (객체를 조립하는 과정)

//실습
//대상 >> 파일 >> 객체 write(직렬화)
//대상 >> 파일 >> 객체 read(역직렬화)

//조건: implements Serializable 클래스만 직렬화 가능

public class UserInfo implements Serializable {
	public String name;
	public String pwd;
	public int age;

	public UserInfo() {}
	public UserInfo(String name, String pwd, int age) {
		this.name = name;
		this.pwd = pwd;
		this.age = age;
	}
	@Override
	public String toString() {
		return "UserInfo [name=" + name + ", pwd=" + pwd + ", age=" + age + "]";
	}
}
```

### ObjectOutputStream
```java
import java.io.BufferedOutputStream;
import java.io.FileOutputStream;
import java.io.ObjectOutputStream;

import kr.or.bit.UserInfo;

//객체를 파일에 write
public class Ex15_ObjectDataOutPutStream {
	public static void main(String[] args) {
		String filename = "UserData.txt";
		try {
			FileOutputStream fos = new FileOutputStream(filename);
			BufferedOutputStream bos = new BufferedOutputStream(fos);

			//객체 (직렬화)
			//write (객체 단위로)
			ObjectOutputStream out = new ObjectOutputStream(bos);
			UserInfo u1 = new UserInfo("superman", "super", 500);
			UserInfo u2 = new UserInfo("scott", "tiger", 50);

			//직렬화
			out.writeObject(u1);
			out.writeObject(u2);

			out.close();
			bos.close();
			fos.close();
			System.out.println("파일생성 -> buffer -> 직렬화객체 -> write");
		}catch(Exception e) {
			System.out.println(e.getMessage());
		}
	}
}
```

## ObjectInputStream
```java
import java.io.BufferedInputStream;
import java.io.FileInputStream;
import java.io.IOException;
import java.io.ObjectInputStream;

import kr.or.bit.UserInfo;

//UserData.txt 파일에서 직렬화된 객체를 read (역직렬화) 다시 조립

public class Ex16_ObjectDataInPutStream {
	public static void main(String[] args) {
		String filename = "UserData.txt";
		FileInputStream fis = null;
		BufferedInputStream bis = null;
		ObjectInputStream in = null;

		try {
			fis = new FileInputStream(filename);
			bis = new BufferedInputStream(fis);
			in = new ObjectInputStream(bis);

			//UserInfo r1 = (UserInfo)in.readObject(); //역직렬화: 복원
			//UserInfo r2 = (UserInfo)in.readObject(); //역직렬화: 복원

			//System.out.println(r1.toString());
			//System.out.println(r2.toString());

			//객체 단의 read 객체가 없으면 null 값 반환
			Object users = null;
			while((users = in.readObject()) != null) {
				System.out.println(users);
			}
		}catch (Exception e) {
			System.out.println(e.getMessage());
		}finally {
			try {
				in.close();
				bis.close();
				fis.close();
			} catch (IOException e) {
				e.printStackTrace();
			}
		}
	}
}
```
