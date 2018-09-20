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

## TCP Echo Server
```java
import java.io.DataInputStream;
import java.io.DataOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.ServerSocket;
import java.net.Socket;

public class Ex03_TCP_Echo_Server {
	public static void main(String[] args) throws Exception {
		ServerSocket serversocket = new ServerSocket(9999);
		System.out.println("접속 대기중...");
		Socket socket = serversocket.accept(); //클라이언트가 접속을 하면
		System.out.println("연결완료");

		//Client write 데이터 서버가 받아서 다시 Client write
		//server >> read, write
		//client >> write, read

		//socket이 가지고 있는 input, output Stream 을 사용

		//read
		InputStream in = socket.getInputStream();
		DataInputStream dis = new DataInputStream(in);

		//write
		OutputStream out = socket.getOutputStream();
		DataOutputStream dos = new DataOutputStream(out);

		while(true) {
			//read
			//client write한 데이터가 있다면
			String clientmsg = dis.readUTF(); //client write하면 동작
			System.out.println("Client Msg :" + clientmsg);

			if(clientmsg.equals("exit")) break; //client "exit" 서버 종료

			//메아리 기능
			dos.writeUTF(clientmsg);
			dos.flush(); //close() 있어도 되는데... 바로 출력하기 위해 flush() 사용
		}
		System.out.println("클라이언트 종료 요청(exit) 서버종료");

		dis.close();
		dos.close();
		in.close();
		out.close();
		socket.close();
		serversocket.close();
	}
}
```

## TCP Echo Client
```java
import java.io.DataInputStream;
import java.io.DataOutputStream;
import java.io.IOException;
import java.net.Socket;
import java.net.UnknownHostException;
import java.util.Scanner;

public class Ex03_TCP_Echo_Client {
	public static void main(String[] args) throws Exception {
		Socket socket = new Socket("192.168.0.24", 9999);
		System.out.println("서버와 연결 되었습니다");

		//read
		DataInputStream dis = new DataInputStream(socket.getInputStream());

		//write
		DataOutputStream dos = new DataOutputStream(socket.getOutputStream());

		Scanner sc = new Scanner(System.in);

		while(true) {
			System.out.println("서버에 전송될 내용 입력: ");
			String msg = sc.nextLine();

			//서버 전송
			dos.writeUTF(msg);
			dos.flush();

			if(msg.equals("exit")) break;

			//서버로부터
			String servermsg = dis.readUTF();
			System.out.println("Echo 메시지: " + servermsg);
		}
		System.out.println("클라이언트 종료");
		dis.close();
		dos.close();
		socket.close();
	}
}
```
