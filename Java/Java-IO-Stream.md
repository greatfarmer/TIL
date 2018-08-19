# IO - Stream
1. stream (연결통로, 입출력통로, 중간매개체)
2. stream 입출력의 크기 (기본적으로 1Byte) >> Byte 입출력 >> Byte[] 배열
3. JAVA API가 제공하는 클래스
  - I/O 작업
  - Byte 처리
  - InputStream(추상), OutputStream(추상)

대상(memory)<br>
ByteArrayInputStream, ByteArrayOutputStream

대상(file)<br>
FileInputStream, FileOutputStream

추가적으로 보조 클래스<br>
Buffer (I/O) 성능향상<br>
BufferedInputStream, BufferedOutputStream 등등...

```java
import java.io.ByteArrayInputStream;
import java.io.ByteArrayOutputStream;
import java.util.Arrays;

public class Ex01_Stream {
	public static void main(String[] args) {
		//Memory (배열)
		//byte (-128 ~ 127): 정수값
		byte[] inSrc = {0,1,2,3,4,5,6,7,8,9};
		//System.out.println(inSrc[5]);
		byte[] outsrc = null; //outsrc 메모리를 가지고 있지 않다

		ByteArrayInputStream input = null; //배열 데이터 read
		ByteArrayOutputStream output = null; //배열 데이터 write

		input = new ByteArrayInputStream(inSrc);
		//생성자 insrc 배열주소 read 할 수 있도록
		output = new ByteArrayOutputStream(); //write 하는 대상 (memory)

		System.out.println("output befre: " + Arrays.toString(outsrc));

		//공식같은 로직
		int data = 0;
		while((data = input.read()) != -1 ) { //더 이상 read() 없다면 return -1
			System.out.println("data: " + data);
			//System.out.println("input.read()" +input.read());
			//input.read() 내부적으로 next() 기능을 가지고 있다

			output.write(data);
			//write 대상: ByteArrayOutputStream객체  read() >> write
		}
		outsrc = output.toByteArray();
		//자기가 가지고 있는 값을 배열로 만들어서
		//bye[] outsrc = null; 에게 주소값 할당
		System.out.println("outsrc: After >" + Arrays.toString(outsrc));
	}
}
```
