# IO - FileReader
- 한글 2Byte -> Stream 통한 통신 불가 (Byte 단위)
- 2Byte 문자 -> Reader, Writer 추상 클래스

## Basic
```java
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;

public class Ex05_Reader_Writer_kor {
	public static void main(String[] args) {
		FileReader fr = null;
		FileWriter fw = null;
		try {
			fr = new FileReader("Ex01_Stream.java");
			fw = new FileWriter("copy_Ex01.txt"); //파일 생성 기능

			int data = 0;
			while((data = fr.read()) != -1) {
				//System.out.println(data + " : " + (char)data);
				//fw.write(data);
				if(data != '\n' && data != '\r' && data != '\t' && data != ' ') {
					fw.write(data); //Compressed data 압축데이터
				}
			}
		}catch(Exception e) {
			System.out.println("문제가 있다");
		}finally {
			try {
				fw.close();
				fr.close();
			} catch (IOException e) {
				e.printStackTrace();
			}
		}
	}
}
```

## Buffer
- 문자 처리 시에도 Buffer 사용 I/O 성능 향상
- Buffer 장점
	- Line 단위 read, writer

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.FileReader;
import java.io.FileWriter;

public class Ex06_FileReader_FileWriter_Buffer {
	public static void main(String[] args){
		/*
		FileWriter fw = new FileWriter("a.txt");
		BufferedWriter bw = new BufferedWriter(fw);
		bw.write("로또");
		bw.newLine(); //장점
		bw.write("1,2,3,4,5");
		bw.flush();
		*/

		FileReader fr = null;
		BufferedReader br = null;

		try {
			fr = new FileReader("Ex01_Stream.java");
			br = new BufferedReader(fr);

			String line = "";
			//br.readLine() >> POINT
			for(int i = 0; (line = br.readLine()) != null; i++) {
				//System.out.println(line);
				if(line.indexOf(";") != -1) {
					System.out.println(line);
				}
			}
		}catch(Exception e) {

		}finally {
			try {
				br.close();
				fr.close();
			} catch (Exception e) {
				e.printStackTrace();
			}

		}
	}
}
```
