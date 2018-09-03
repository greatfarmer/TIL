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
