# IO - Stream File

## Basic
```java
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;

public class Ex03_Stream_File {
	public static void main(String[] args) throws Exception {
		String orifile = "C:\\Temp\\1.png";
		String targetfile = "copy.png";

		FileInputStream fis = new FileInputStream(orifile);
		FileOutputStream fos = new FileOutputStream(targetfile);

		int data=0;
		while((data = fis.read()) != -1) {
			fos.write(data);
		}
		fis.close();
		fos.close();
		System.out.println("작업완료");
	}
}
```

## Buffer
컴퓨터 느린 작업 (DISK 파일 read, write) <br>
File 작업 (DISK) (read, write) byte 단위 <br>
위 같은 문제를 해결하기 위해서 <br>
Buffer 메모리 (작접 해 둔것을) <br>
**file 작업할때 중간에 Buffer 사용** <br>

File I/O 성능개선 <br>
Line 단위 처리

JAVA API
보조 스트림(input, output 추상클래스 상속한 클래스가 객체로 존재해야지만 사용가능) <br>
- BufferedInputStream
- BufferedOutputStream

```java
import java.io.BufferedInputStream;
import java.io.BufferedOutputStream;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;

public class Ex04_Stream_File_Buffer {
	public static void main(String[] args) {
		FileOutputStream fos = null;
		BufferedOutputStream bos = null;

		try {
			fos = new FileOutputStream("data.txt"); //파일 생성 기능 가지고 있다
			bos = new BufferedOutputStream(fos);

			for(int i = 0; i < 10; i++) {
				bos.write('A');
			}
			/*
			java buffer 크기: 8kByte => 8192Byte
			1.buffer 안에 내용이 8kByte 채우면 자동으로 비운다 (요청자에게 준다)
			2.buffer 강제로 비우는 방법: flush() 강제호출
			3.bos.close(); 내부에서 flush() 호출
			*/
			//bos.flush();
		}catch(Exception ex) {
			System.out.println(ex.getMessage());
		}finally {
			try {
				bos.close(); //buffer close(flush() 포함)
				fos.close();
			}catch (IOException e) {
				e.printStackTrace();
			}
		}
	}
}
```
