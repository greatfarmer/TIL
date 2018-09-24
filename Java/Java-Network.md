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

## TCP Multi Chat Server
```java
import java.io.DataInputStream;
import java.io.DataOutputStream;
import java.net.ServerSocket;
import java.net.Socket;
import java.util.HashMap;
import java.util.Iterator;

//다중 채팅
//서버 1개
//여러명의 클라이언트 접속
//클라이언트 1명( socket )
public class Ex05_TCP_Multi_Chat_Server {

	ServerSocket serversocket = null;
	Socket socket = null;
	//KEY POINT
	HashMap<String,DataOutputStream> ClientMap;

	Ex05_TCP_Multi_Chat_Server(){
		//각 클라이언트 (socket 가지는 출력객체를 관리)
		ClientMap = new HashMap<>();
	}
	//1. 서버 초기화 작업
	public void init() {
		try {
			serversocket = new ServerSocket(9999);
			System.out.println("[서버시작 ... 응답대기 ...]");
			while(true) {
			socket = serversocket.accept();
			System.out.println("[" + socket.getInetAddress() + "/"
			                  + socket.getPort() + "]");
			//코드작업
			//클라이언트 접속시 마다 ....
				Thread client = new MultiServerRev(socket);
				client.start();
			//
			}


		}catch (Exception e) {
			System.out.println("Init() : " + e.getMessage());
		}
	}

	//2. 접속된 모든 클라이언트에게 (socket) 메시지를 전달하는 함수
	//Map(key ,value)
	//key => 사용자ID (고유한 값) ex) kglim , hong , kim , lee ..
	//ClientMap<"hong" 각각의  socket객체에서 얻어낸 DataOutPutStream객체 저장>)
	//ClientMap<"kim"  각각의  socket객체에서 얻어낸 DataOutPutStream객체 저장>)
	void sendAllMsg(String msg) {
		Iterator<String> ClientKeySet = ClientMap.keySet().iterator();
		while(ClientKeySet.hasNext()) {
			try {
				DataOutputStream clientout = ClientMap.get(ClientKeySet.next());
				//각각의 접속한 client 에게 메시지 출력
				clientout.writeUTF(msg);
			}catch(Exception e) {
				System.out.println("Send AllMsg :" +  e.getMessage());
			}
		}
	}

	//3. 접속된 유저 리스트 목록 관리하기
	String showUserList(String name) {
		//문자열 누적 (String name ="김" ;  name+="박" ; name+="이"
		//대체 해서 : StringBuilder
		StringBuilder output = new StringBuilder("<<접속자 목록>>\n\r");
		Iterator<String> users = ClientMap.keySet().iterator();
		while(users.hasNext()) {
			try {
				String key = users.next(); //kim , park ,,,,,(접속한 ID)
				if(key.equals(name)) { //목록을 요청한 ID
					key+="(*)";
				}
				output.append(key+"\n\r");

			}catch(Exception e) {
				System.out.println("showUserList 예외 : " + e.getMessage());
			}
		}
		output.append("<<" + ClientMap.size() + ">>" + "명 접속중 \n\r");
		return output.toString();
	}

	//4. 궛속말 기능
	void SendToMsg(String fromname, String toname , String toMsg) {
		try {
			 ClientMap.get(toname).writeUTF("귓속말 from (" + fromname +")=>"+ toMsg);
			 ClientMap.get(fromname).writeUTF("귓속말 to (" + toname +")=>"+ toMsg);
		}catch (Exception e) {
			System.out.println("SendToMsg 예외 : " + e.getMessage());
		}
	}

	//Thread 사용 (보내기 , 받기)
	//inner class 형태
	class MultiServerRev extends Thread{
		Socket socket = null;
		DataInputStream in;
		DataOutputStream out;

		public MultiServerRev(Socket socket) {
			this.socket = socket;
			try {
				in = new DataInputStream(this.socket.getInputStream());
				out = new DataOutputStream(this.socket.getOutputStream());
			}catch(Exception e) {
				System.out.println("MultiServerRev 예외 :" + e.getMessage());
			}
		}

		@Override
		public void run() {
			//기본 클라이언트 : in.readUTF() , out.WriteUTF()
			String name=""; //클라이언트 이름 저장(key)
			try {
					//연결된 socket을 통해서 Client 메시지 전달
					out.writeUTF("이름을 입력하세요");
					name = in.readUTF();
					out.writeUTF("현재 채팅방에 입장하셨습니다");

					//서버에 접속된 모든 사용자에(socket) 입력된 내용 전달
					sendAllMsg(name +"님 입장^^");

					//KEY POINT
					ClientMap.put(name, out); //Map(이름, 각각 socket 출력객체)
					System.out.println("서버 모니터링 : 현재 접속자는 수 " + ClientMap.size() +"명");

					//추가기능(대화)
					while(in != null) {
						String msg = in.readUTF();

						//채팅장에 참여하고 있는 접속자에게 전달
						if(msg.startsWith("/")) {
							if(msg.trim().equals("/접속자")) {
								out.writeUTF(showUserList(name)); //접속자 목록 출력
							}else if(msg.startsWith("/귓속말")) {
								String[] msgArr = msg.split(" ",3); // /귓속말 홍길동 방가방가
								if(msgArr == null || msgArr.length < 3) {
									out.writeUTF("HELP:사용법\n\r /귓속말 [상대방이름] [메시지]");
								}else {
									String toName = msgArr[1];
									String toMsg  = msgArr[2];
									if(ClientMap.containsKey(toName)) {
										//메시지 보내기
										//특정 상대에게 메시지 보내기
										SendToMsg(name, toName, toMsg);
									}else {
										out.writeUTF("입력한 사용자가 없습니다");
									}
								}
							}else {
								out.writeUTF("잘못된 명령어 입니다");
							}
						}else {
							//전체 사용자에게 write
							sendAllMsg("[" + name + "]" + msg);
						}
					}//while end

			}catch (Exception e) {
				System.out.println("Thread run 예외 :" + e.getMessage());
			}finally {
				//Client 종료 (퇴장)
				ClientMap.remove(name);
				sendAllMsg(name + "님 퇴장하였습니다");
				System.out.println("서버 모니터링 : 현재 접속자는 " + ClientMap.size() +"명" );
			}
		}
	}

	public static void main(String[] args) {
		Ex05_TCP_Multi_Chat_Server server = new Ex05_TCP_Multi_Chat_Server();
		server.init();
	}
}
```

## TCP Multi Chat Client
```java
import java.awt.Color;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.io.DataInputStream;
import java.io.DataOutputStream;
import java.net.Socket;

import javax.swing.JFrame;
import javax.swing.JScrollPane;
import javax.swing.JTextArea;
import javax.swing.JTextField;
import javax.swing.ScrollPaneConstants;

public class Ex05_TCP_Multi_Chat_Client extends JFrame implements ActionListener , Runnable {
	JTextArea ta;//출력창(다중라인)
	JTextField txtinput;//입력창
	DataInputStream in;
	DataOutputStream out;

	Ex05_TCP_Multi_Chat_Client(){
		//UI 구성
		this.setTitle("Multi채팅");
		ta = new JTextArea();
		txtinput = new JTextField();
		JScrollPane sp =  new JScrollPane(ta,ScrollPaneConstants.VERTICAL_SCROLLBAR_ALWAYS,
				                          ScrollPaneConstants.HORIZONTAL_SCROLLBAR_NEVER);
		sp.setAutoscrolls(true);
		ta.setBackground(Color.WHITE);
		ta.setLineWrap(true);//텍스트 자동 줄바꿈
		ta.setEditable(false);//편집 안되요

		txtinput.setText("이름 입력");
		txtinput.requestFocus();
		txtinput.selectAll();

		this.add(sp,"Center");
		this.add(txtinput, "South");
		this.setSize(400, 500);
		this.setVisible(true);

		this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);

		//TextField 이벤트 처리 (엔터)
		txtinput.addActionListener(this);

		//소켓 설정하기
		try {
			 Socket socket = new Socket("192.168.0.24", 9999);
			 in = new DataInputStream(socket.getInputStream());
			 out = new DataOutputStream(socket.getOutputStream());
			 //서버와 연결
			 ta.append("서버에 접속되었습니다\n\r");

			 //Thread 구동
			 Thread client = new Thread(this);
			 client.start();
			 ////////////
		}catch(Exception e) {
			System.out.println(e.getMessage());
		}

	}

	@Override
	public void actionPerformed(ActionEvent e) {
		//TextField 입력하고 Enter 처리하면
		//txtinput.addAction...(this)
		if(e.getSource().equals(txtinput)) {
			String msg = txtinput.getText();
			try {
				out.writeUTF(msg); //서버로 출력
				//System.out.println("입력값 : "+msg);
				//ta.setText(msg);
				txtinput.setText("");
			}catch(Exception ex) {
				System.out.println(ex.getMessage());
			}
		}
	}

	@Override
	public void run() {
		try {
			String msg = in.readUTF();
			ta.append(msg + "\n\r");
			while(in != null) {
				msg = in.readUTF();
				ta.append(msg +"\n\r");
			}
		}catch (Exception e) {
			System.out.println("접속중 서버와 연결 종료");
			return;
		}

	}

	public static void main(String[] args) {
		Ex05_TCP_Multi_Chat_Client client = new Ex05_TCP_Multi_Chat_Client();
	}
}
```
