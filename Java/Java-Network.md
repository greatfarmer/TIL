# Network

## URLConnection
네트워크 작업을 할 수 있는 자원 <br>
JAVA API 제공 <br>
클래스를 통해서 <br>
URL 클래스 (인터넷 상에 주소를 다루는 클래스) <br>
URLConnection 클래스(연결된 URL 주소로 부터 다양한 정보와 작업)

```java
import java.io.BufferedInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.net.URL;
import java.net.URLConnection;

public class Ex01_URLConnection {
	public static void main(String[] args) throws IOException {
		String urlstr = "http://www.kangcom.com/images/banner/chocolate_150.jpg";
		URL url = new URL(urlstr);
		BufferedInputStream bis = new BufferedInputStream(url.openStream());

		FileOutputStream fos = new FileOutputStream("copy1.jpg"); //파일 생성 (프로젝트 폴더)

		byte[] buffer = new byte[2048];
		int n = 0;
		int count = 0;

		URLConnection uc = url.openConnection();
		int filesize = uc.getContentLength();

		System.out.println("파일크기: " + filesize);
		System.out.println("파일형식: " + uc.getContentType());

		while((n = bis.read(buffer)) != -1) {
			fos.write(buffer, 0, buffer.length);
			fos.flush();
			System.out.println("n: " + n);
			count += 1;
		}
		System.out.println("count: " + count);
		fos.close();
		bis.close();
	}
}
```

## TCP Server
```java
import java.io.DataOutputStream;
import java.io.IOException;
import java.io.OutputStream;
import java.net.ServerSocket;
import java.net.Socket;

//TCP 서버
//server(process) - client(process)

//서버: 192.168.0.24
//포트: port: 9999

public class Ex02_TCP_Server {
	public static void main(String[] args) throws IOException {
		ServerSocket serversocket = new ServerSocket(9999);
		System.out.println("접속 대기중...");
		Socket socket = serversocket.accept(); //클라이언트가 접속을 하면
		System.out.println("연결완료");

		//연결이 되면
		//서버: 클라이언트 (read, write)
		//socket과 socket
		//socket(input, output Stream)

		OutputStream out = socket.getOutputStream();
		DataOutputStream dos = new DataOutputStream(out); //8가지 기본 타입 + @
		dos.writeUTF("문자데이터 Byte 통신 가능: 아뇨, 전 뚱인데요");

		System.out.println("서버 종료");

		dos.close();
		out.close();
		socket.close();
		serversocket.close();
	}
}
```

## TCP Client
```java
import java.io.DataInputStream;
import java.io.IOException;
import java.io.InputStream;
import java.net.Socket;

//client

//server IP: 192.168.0.24
//port: 9999
public class Ex02_TCP_Client {
	public static void main(String[] args) throws Exception, IOException {
		Socket socket = new Socket("192.168.0.24", 9999);
		System.out.println("서버와 연결 되었습니다");

		//서버에서 보낸 메시지 받기
		InputStream in = socket.getInputStream();
		DataInputStream dis = new DataInputStream(in);

		String servermsg = dis.readUTF();
		System.out.println("서버에서 보낸 메시지: " + servermsg);

		dis.close();
		in.close();
		socket.close();
	}
}
```
