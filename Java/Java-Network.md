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

## TCP Chat Server
```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.net.ServerSocket;
import java.net.Socket;

//Echo: 순차적인 데이터 처리 (Ex01 ~ Ex03)

//Server: read, write
//Client: read, write
//동시에 (Thread) ...
//read (Thread) >> stack, write (Thread) >> stack

//1:1 채팅은 입력과 출력이 동시에...
public class Ex04_TCP_Chat_Server {
	public static void main(String[] args) {
		ServerSocket serversocket = null;

		try {
			serversocket = new ServerSocket(9999);
			System.out.println("접속 대기중...");
			Socket socket = serversocket.accept(); //클라이언트가 접속을 하면
			System.out.println("연결완료");

			//ServerSend 객체나
			//ServerReceive 객체나 socket 객체의 주소가 필요
			//WHY?: socket을 통해서 read, write

			//ServerSend send = new ServerSend(socket);
			//send.start();
			new ServerSend(socket).start();

			//ServerReceive receive = new ServerReceive();
			//receive.start();
			new ServerReceive(socket).start();

		}catch (Exception e) {
			System.out.println(e.getMessage());
		}finally {
			try {
				serversocket.close();
			}catch (Exception e) {
				System.out.println(e.getMessage());
			}
		}
	}
}

//고민: socket (read, write 객체)
//Write: socket > outputStream
class ServerSend extends Thread {
	//cmd 창에서 입력한 값을 read해서
	//OutputStream 사용해서 write >> 연결된 socket을 통해서
	Socket socket;
	public ServerSend(Socket socket) {
		this.socket = socket;
	}

	@Override
	public void run() { //다른 stack main 함수와 같은 역할
		BufferedReader br = null;
		PrintWriter pw = null;
		try {
			/*
			Scanner sc = new Scanner(System.in);
			String msg = sc.nextLine();
			OutputStream out = socket.getOutputStream();
			DataOutputStream dos = new DataOutputStream(out);
			dos.writeUTF(msg);
			*/

			//Buffer >> Line() read
			br = new BufferedReader(new InputStreamReader(System.in));
			// InputStreamReader(System.in)은 Scanner(System.in) 역할을 한다
			pw = new PrintWriter(socket.getOutputStream(), true); // true하면 자동 flush()

			while(true) {
				String data = br.readLine(); //sc.nextline() 역할
				if(data.equals("exit")) break;
				pw.println(data); //접속한 클라이언트에게 메시지 전송 dos.writeUTF(msg) 역할
			}
			System.out.println("server send 작업 종료");
		}catch (Exception e) {
			System.out.println(e.getMessage());
		}finally {
			try {
				pw.close();
				br.close();
			}catch(Exception e) {
				System.out.println(e.getMessage());
			}
		}
	}
}

//Read: socket > inputStream
class ServerReceive extends Thread {
	//연결된 socket을 통해서 read해서
	//cmd 그 결과를 출력
	Socket socket;
	public ServerReceive(Socket socket) {
		this.socket = socket;
	}
	@Override
	public void run() {		
		BufferedReader br = null;
		try {
			/*
			InputStream in = socket.getInputStream();
			DataInputStream dis = new DataInputStream(in);
			String msg = dis.readUTF();
			*/
			br = new BufferedReader(new InputStreamReader(socket.getInputStream()));
			String data = null;
			while((data = br.readLine()) != null) {
				System.out.println("Client로부터 받은 메시지 [ " + data + " ]");
			}
			System.out.println("ServerReceive 종료");
		}catch (Exception e) {
			System.out.println(e.getMessage());
		}finally {
			try {
				br.close();
			}catch (Exception e) {
				System.out.println(e.getMessage());
			}
		}
	}
}
```

## TCP Chat Client
```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.net.Socket;

//Inner Class 사용해서 구현
//Clientsend
//ClientReceive

public class Ex04_TCP_Chat_Client {
	public static void main(String[] args) {
		Ex04_TCP_Chat_Client client = new Ex04_TCP_Chat_Client();
	}

	Socket socket = null;
	public Ex04_TCP_Chat_Client() {
		try {
			socket = new Socket("192.168.0.24", 9999);
			System.out.println("서버와 연결 되었습니다");

			new ClientSend().start();
			new ClientReceive().start();
		}catch(Exception e) {
			System.out.println(e.getMessage());
		}
	}

	class ClientSend extends Thread {
		@Override
		public void run() {
			BufferedReader br = null;
			PrintWriter pw = null;
			try {
				br = new BufferedReader(new InputStreamReader(System.in));
				pw = new PrintWriter(socket.getOutputStream(), true);

				while(true) {
					String data = br.readLine();
					if(data.equals("exit")) break;
					pw.println(data);
				}
				System.out.println("client send 작업 종료");
			}catch (Exception e) {
				System.out.println(e.getMessage());
			}
		}
	}

	class ClientReceive extends Thread {
		@Override
		public void run() {		
			BufferedReader br = null;
			try {
				br = new BufferedReader(new InputStreamReader(socket.getInputStream()));
				String data = null;
				while((data = br.readLine()) != null) {
					System.out.println("Server로부터 받은 메시지 [ " + data + " ]");
				}
				System.out.println("ClientReceive 종료");
			}catch (Exception e) {
				System.out.println(e.getMessage());
			}finally {
				try {
					br.close();
				}catch (Exception e) {
					System.out.println(e.getMessage());
				}
			}
		}
	}
}
```
