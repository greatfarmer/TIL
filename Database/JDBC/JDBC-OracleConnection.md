# [JDBC] Oracle Connection
```java
/*
JDBC
1. Java를 통해서 Oracle 연결하고 CRUD 작업
2. 어떠한 DB 소프트웨어 사용 결정 (Oracle, mysql, ms-sql)
2.1 제품에 맞는 드라이버 필요 (각 밴더 사이트에서 다운로드 받아서 사용)
2.2 오라클 (로컬 PC 오라클 DB 서버 설치) >> ojdbc6.jar (드라이버 파일)

3. cmd 기반의 Java Project 에서는 드라이버 사용하기 위해서 참조
3.1 Java Build Path (jar 추가) 하는 작업
3.2 드라이버 사용준비 완료 >> 드라이버 사용할 수 있도록 메모리 (new ..)
3.3 Class.forName("class 이름") >> new 통일한 효과

4. JAVA (JDBC API)
4.1 import java.sql.*; 제공하는 자원 (대부분의 자원은 interface, class)
4.2 개발자는 interface를 통해서 작업 (궁금증: why interface일까? hint)java 뿐만 아니라 다양한 언어 사용) >> 다형성

5. DB연결 -> 명령 -> 실행 -> 처리 -> 자원해제
5.1 명령 (CRUD): select, insert, update, delete
5.2 처리: select 화면 출력할꺼야 아니야 난 확인만...
5.3 자원해제 (성능)

*연결 문자열 (ConnectionString) 설정
채팅 (client -> server 연결하기 위해서)
네트워크 DB (서버 IP, PORT, SID(전역 데이터베이스 이름), 접속계정, 접속비밀번호)
 */

import java.sql.*;

public class Ex01_Oracle_Connection {
	public static void main(String[] args) throws Exception {
		//DB연결 -> 명령 -> 실행 -> 처리 -> 자원해제
		Class.forName("oracle.jdbc.OracleDriver"); //new memory 드라이버 로드
		System.out.println("오라클 드라이버 메모리 로딩");

		Connection conn = DriverManager.getConnection("jdbc:oracle:thin:@localhost:1521:xe", "user", "1004");
		//연결 객체의 주소값을 리턴 받았어요
		System.out.println(conn.isClosed() + "(False)");

		Statement stmt = conn.createStatement(); //명령 객체 획득 (연결기반에서)

		//CRUD
		String sql = "select empno, ename, sal from emp";

		ResultSet rs = stmt.executeQuery(sql);

		//처리
		while(rs.next()) {
			System.out.println(rs.getInt("empno") + "/" +
								rs.getString("ename") + "/" +
								rs.getInt("sal"));
		}

		//연결해제
		rs.close();
		conn.close();
		System.out.println("DB연결: " + conn.isClosed());
	}
}
```
