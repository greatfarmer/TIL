# IO - FileReader
한글 2Byte -> Stream 통한 통신 불가 (Byte 단위) <br>
2Byte 문자 -> Reader, Writer 추상 클래스

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
