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
