# [JDBC] MySQL Connection
```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.Statement;
import java.util.Scanner;

/*
1. 드라이버 참조 (프로젝트에 jar 파일 추가)
2. 드라이버 로딩 (JVM: Class.forName(""))
3. 연결 객체 생성 -> DriverManager 클래스...
4. 명령 객체 생성 -> CRUD 작업 준비 (Statement)
5. 명령 실행 -> DQL(select 데이터 1건, select 모든 row) => 결과 집합 생성(ResultSet)
			DML(insert, delete, update) => 결과 집합 생성 x => 성공 여부
6. 명령 처리 (2가지): DQL > 조회(출력), DML > 결과에 따른 로직 (성공/실패)
7. 자원해제

JDBC API
(인터페이스 기반)
Connection
Statement
PrepareStatement
ResultSet
:다형성 적용 코드 (어떤 DB를 작업하던 표준화된 코드로 작업)
 */

public class Ex02_Mysql_Connection {
	public static void main(String[] args) {
		/*
		Class.forName("com.mysql.jdbc.Driver"); //new memory 드라이버 로드
		//jdbc:mysql://localhost:3306/user
		Connection conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/user","user","1004");
		System.out.println(conn.isClosed());
		*/
		Connection conn = null;
		Statement stmt = null;
		ResultSet rs = null;

		try {
			//2. 드라이버 로딩
			Class.forName("com.mysql.jdbc.Driver");

			//3. 연결 객체 생성 (주소값 할당 받기)
			conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/user","user","1004");

			//4. 명령 객체 생성
			stmt = conn.createStatement();

			//4.1 실행할 자원 (Query 문장)
			String job = "";
			Scanner sc = new Scanner(System.in);
			System.out.println("직종입력:");
			job = sc.nextLine();
			//where job = 'CLERK'
			//select empno, ename, job from emp where job = 'CLERK'
			String sql = "select empno, ename, job from emp where job = '" + job + "'";

			//5. 명령 실행
			//DQL: 실행하는 함수 달라요 > stmt.executeQuery(sql) > return ResultSet 객체의 주소
			//DML: 실행하는 함수 달라요 > stmt.executeUpdate(sql) > 결과 집합 (x) 출력 데이터(x)
			//executeUpdate() 함수가 return하는 자원은 반영된 행의 개수: 성공....실패....로직에 사용됨
			//delete from emp where deptno = 30 >> 5건 지워지면 return 5 (삭제된 행의 개수)

			rs = stmt.executeQuery(sql);

			//6. 명령 처리
			//DQL: 1. 결과가 없는 경우 (where empno = 5555)
			//	   2. 결과가 1건인 경우(PK, UNIQUE) 조건 조회 >> where empno = 7788
			//	   3. 결과가 여러개의 row >> select * from emp where deptno = 10

			/*
			while(rs.next()) {
				System.out.println(rs.getInt("empno") + "," +
								   rs.getString("ename") + "," +
						           rs.getString("job"));
			}

			1. 간단하다 (단순)
			2. 결과 집합이 없는 경우에 대한 처리가 안되요
			*/

			/*
			if(rs.next()) {
				System.out.println(rs.getInt("empno") + "," +
								   rs.getString("ename") + "," +
						           rs.getString("job"));
			}else {
				System.out.println("조회된 데이터가 없습니다");
			}

			1. Multi Row READ가 안된다
			2. 결과 집합이 없는 경우에 대한 처리가 가능하다
			*/

			//위 두 개 장점 (공식같은 로직)
			//1. 결과 집합이 없는 경우 처리, 2. Single row, 3. Multi row 가능
			if(rs.next()) {
				do {
					System.out.println(rs.getInt("empno") + "," +
									   rs.getString("ename") + "," +
							           rs.getString("job"));
				}while(rs.next());
			}else {
				System.out.println("조회된 데이터가 없습니다");
			}

		}catch (Exception e) {
			System.out.println(e.getMessage());
		}finally {
			/*
			try {
				//자원해제
				rs.close();
				stmt.close();
				conn.close(); //반드시 끊어야한다.
			}catch (Exception e2) {
				e2.printStackTrace();
			}
			*/

			//정상적인 구문
			if(rs != null) try { rs.close(); }catch (Exception e2) {}
			if(stmt != null) try { stmt.close(); }catch (Exception e2) {}
			if(conn != null) try { conn.close(); }catch (Exception e2) {}
		}
	}
}
```
