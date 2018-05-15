# PL-SQL
```sql
--PL-SQL
--PL/SQL 은 Oracle's Procedural Language extension to SQL. 의 약자 입니다.
--SQL문장에서 변수정의, 조건처리(IF), 반복처리(LOOP, WHILE, FOR)등을 지원하며,
--오라클 자체에 내장되어 있는Procedure Language입니다
--DECLARE문을 이용하여 정의되며, 선언문의 사용은 선택 사항입니다.
--PL/SQL 문은 블록 구조로 되어 있고PL/SQL 자신이 컴파일 엔진을 가지고 있습니다.

--Tool > 보기 > DBMS 출력창 > + 버튼 클릭 > 사용자 접속(개발자)
--DBMS 출력창 : 이클립스 console 창

--pl-sql (java : System.out.println()) 결과 확인
--DBMS 출력 창에서

--0. 사전 table 작성
CREATE TABLE EMP
(EMPNO number not null,
ENAME VARCHAR2(10),
JOB VARCHAR2(9),
MGR number ,
HIREDATE date,
SAL number ,
COMM number ,
DEPTNO number );
--alter session set nls_date_format='YYYY-MM-DD HH24:MI:SS';

INSERT INTO EMP VALUES
(7369,'SMITH','CLERK',7902,'1980-12-17',800,null,20);
INSERT INTO EMP VALUES
(7499,'ALLEN','SALESMAN',7698,'1981-02-20',1600,300,30);
INSERT INTO EMP VALUES
(7521,'WARD','SALESMAN',7698,'1981-02-22',1250,200,30);
INSERT INTO EMP VALUES
(7566,'JONES','MANAGER',7839,'1981-04-02',2975,30,20);
INSERT INTO EMP VALUES
(7654,'MARTIN','SALESMAN',7698,'1981-09-28',1250,300,30);
INSERT INTO EMP VALUES
(7698,'BLAKE','MANAGER',7839,'1981-04-01',2850,null,30);
INSERT INTO EMP VALUES
(7782,'CLARK','MANAGER',7839,'1981-06-01',2450,null,10);
INSERT INTO EMP VALUES
(7788,'SCOTT','ANALYST',7566,'1982-10-09',3000,null,20);
INSERT INTO EMP VALUES
(7839,'KING','PRESIDENT',null,'1981-11-17',5000,3500,10);
INSERT INTO EMP VALUES
(7844,'TURNER','SALESMAN',7698,'1981-09-08',1500,0,30);
INSERT INTO EMP VALUES
(7876,'ADAMS','CLERK',7788,'1983-01-12',1100,null,20);
INSERT INTO EMP VALUES
(7900,'JAMES','CLERK',7698,'1981-10-03',950,null,30);
INSERT INTO EMP VALUES
(7902,'FORD','ANALYST',7566,'1981-10-3',3000,null,20);
INSERT INTO EMP VALUES
(7934,'MILLER','CLERK',7782,'1982-01-23',1300,null,10);

COMMIT;

CREATE TABLE DEPT
(DEPTNO number,
DNAME VARCHAR2(14),
LOC VARCHAR2(13) );

INSERT INTO DEPT VALUES (10,'ACCOUNTING','NEW YORK');
INSERT INTO DEPT VALUES (20,'RESEARCH','DALLAS');
INSERT INTO DEPT VALUES (30,'SALES','CHICAGO');
INSERT INTO DEPT VALUES (40,'OPERATIONS','BOSTON');

COMMIT;



CREATE TABLE SALGRADE
( GRADE number,
LOSAL number,
HISAL number );

INSERT INTO SALGRADE VALUES (1,700,1200);
INSERT INTO SALGRADE VALUES (2,1201,1400);
INSERT INTO SALGRADE VALUES (3,1401,2000);
INSERT INTO SALGRADE VALUES (4,2001,3000);
INSERT INTO SALGRADE VALUES (5,3001,9999);
COMMIT;

select * from emp;
desc emp;

SELECT COUNT(*) CNT
FROM EMP;

select * from emp;
select empno from emp;

INSERT INTO EMP(EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO)
VALUES (1001, 'KANG', 'DEVELOPER', 1000, '2018-05-04', 9000, 5000, 04);

SELECT COUNT(*) CNT FROM EMP WHERE DEPTNO=10;
--------------------------------------------------------------------------------

--1.pl-sql 블럭 단위 실행
BEGIN
  DBMS_OUTPUT.PUT_LINE('HELLO WORLD');
END;

--pl-sql
--선언부(변수)
--실행부(변수 값을 할당 , 제어구문)
--예외부(Exception)

DECLARE --선언
  vno number(4);
  vname varchar2(20);
BEGIN
  vno := 100; -- 할당 >  String s; s = "홍길동"
  vname := 'kglim';
  DBMS_OUTPUT.PUT_LINE(vno); --화면 출력
  DBMS_OUTPUT.PUT_LINE(vname || '입니다');
END;

--변수 선언 방법 (타입)
--DECLARE
--v_job varchar2(10);
--v_count number ;= 10; --초기값 설정
--v_date date := sysdate + 7; --초기값 설정
--v_valid boolean not null := true
--------------------------------------------------------------------------------
DECLARE
  vno number(4);
  vname varchar2(20);
BEGIN
   select empno ,ename
      into vno , vname --pl-sql 사용하는 구분 (into) . 실행결과 변수에 담기
   from emp
   where empno=&empno; --& 자바 scanner  역활 (입력값 받기)

   DBMS_OUTPUT.PUT_LINE('변수값 : ' || vno || '/' || vname);
END;

--실습 테이블 만들기
create table pl_test(
no number , name varchar2(20) , addr varchar2(50));

DECLARE
  v_no number := '&NO';
  v_name varchar2(20) := '&NAME';
  v_addr varchar2(50) := '&ADDR';
BEGIN
  insert into pl_test(no,name,addr)
  values(v_no , v_name , v_addr);
  commit;
END;

select * from pl_test;  
--변수 제어하기(타입)
--1.1 타입 : v_empno number(10)
--1.2 타입 : v_empno emp.empno%TYPE  (emp 테이블에 있는 empno 컬럼의 타입 사용)
--1.3 타입 : v_row emp%ROWTYPE (v_row 변수는 emp 테이블 모든 컬럼 타입 정보)

--QUIZ
--두개의 정수를 입력받아서 그 합을 출력
DECLARE
  v_no1 number := '&no1';
  v_no2 number := '&no2';
  result number;
BEGIN
    result := v_no1 + v_no2;
    DBMS_OUTPUT.PUT_LINE('result : ' || result);
END;

--------------------------------------------------------------------------------
DECLARE
  v_emprow emp%ROWTYPE;
BEGIN
  select *
    into v_emprow -- empno , ename , ,..... deptno
  from emp
  where empno=7788;

  DBMS_OUTPUT.PUT_LINE(v_emprow.empno || '-' || v_emprow.ename);
END;

--------------------------------------------------------------------------------
create sequence empno_seq
increment by 1
start with 8000
maxvalue 9999
nocycle
nocache;

DECLARE
  v_empno emp.empno%TYPE;
BEGIN
  select empno_seq.nextval
   into v_empno
  from dual;

  insert into empdml(empno ,ename)
  values(v_empno,'홍길동');
  commit;
END;

select * from empdml;
--여기까지 변수 선언 , 타입 , 값 할당
--------------------------------------------------------------------------------
--pl-sql 제어문
DECLARE
  vempno emp.empno%TYPE;
  vename emp.ename%TYPE;
  vdeptno emp.deptno%TYPE;
  vname varchar2(20) := null;
BEGIN
  select empno , ename , deptno
    into vempno , vename , vdeptno
  from emp
  where empno=7788;
  --제어문 if(조건문){실행문}
  IF(vdeptno = 10) THEN vname := 'ACC'; -- if(vdeptno==10) { vname = "ACC"}
    ELSIF(vdeptno=20) THEN vname := 'IT';
    ELSIF(vdeptno=30) THEN vname := 'SALES';
  END IF;
  DBMS_OUTPUT.PUT_LINE('당신의 직종은 : ' || vname);
END;

--IF() THEN 실행문
--ELSIF() THEN 실행문
--ELSE 실행문
--사번이 7788번인 사원의 사번 , 이름 , 급여를 변수에 담고
--변수에 담긴 급여가 2000 이상이면 '당신의 급여는 BIG' 출력하고
--그렇지 않으면(ELSE) '당신의 급여는 SMALL' 이라고 출력하세요

DECLARE
  vempno emp.empno%TYPE;
  vename emp.ename%TYPE;
  vsal   emp.sal%TYPE;
BEGIN
  select empno , ename , sal
    into vempno , vename , vsal
  from emp
  where empno=7788;
  --제어문 if(조건문){실행문}
    IF(vsal >  2000) THEN
         DBMS_OUTPUT.PUT_LINE('당신의 급여는 BIG ' || vsal);
    ELSE
         DBMS_OUTPUT.PUT_LINE('당신의 급여는 SMALL ' || vsal);
    END IF;
 END;

 -------------------------------------------------------------------------------
 --CASE
DECLARE
  vempno emp.empno%TYPE;
  vename emp.ename%TYPE;
  vdeptno emp.deptno%TYPE;
  v_name varchar2(20);
BEGIN
     select empno, ename , deptno
        into vempno, vename , vdeptno
     from emp
     where empno=7788;

--    v_name := CASE vdeptno
--                WHEN 10  THEN 'AA'
--                WHEN 20  THEN 'BB'
--                WHEN 30  THEN 'CC'
--                WHEN 40  THEN 'DD'
--              END;

    v_name := CASE
                WHEN vdeptno=10  THEN 'AA'
                WHEN vdeptno in(20,30)  THEN 'BB'
                WHEN vdeptno=40  THEN 'CC'
                ELSE 'NOT'
              END;
    DBMS_OUTPUT.PUT_LINE('당신의 부서명:' || v_name);            
END;
--------------------------------------------------------------------------------
--pl-sql (반복문)
--Basic loop
/*
LOOP
  문자;
  EXIT WHEN (조건식)
END LOOP
*/
DECLARE
  n number :=0;
BEGIN
  LOOP
    DBMS_OUTPUT.PUT_LINE('n value : ' || n);
    n := n + 1;
    EXIT WHEN n > 5;
  END LOOP;
END;

/*
WHILE(n < 6)
LOOP
   실행문;
END LOOP
*/
DECLARE
  num number := 0;
BEGIN
  WHILE(num < 6)
  LOOP
    DBMS_OUTPUT.PUT_LINE('num 값 : ' || num);
    num := num +1;
  END LOOP;
END;

--for
--java for(int i = 0 ; i < 10 ; i++) {}
BEGIN
  FOR i IN 0..10 LOOP
    DBMS_OUTPUT.PUT_LINE(i);
  END LOOP;
END;

--위 FOR 문을 사용해서 (1~100까지 합) 구하세요
DECLARE
total number :=0;
BEGIN
  FOR i IN 1..100 LOOP
    total := total + i;
  END LOOP;
  DBMS_OUTPUT.PUT_LINE('1~100 총합 : ' || total);
END;

--11g 이전 (continue (x))
--11g (continue 추가)
DECLARE
  total number := 0;
BEGIN
  FOR i IN 1..100 LOOP
    DBMS_OUTPUT.PUT_LINE('변수 : ' || i);
    CONTINUE WHEN i > 5; --skip
    total := total + i; -- 1 , 2 , 3 , 4, 5
  END LOOP;
    DBMS_OUTPUT.PUT_LINE('합계 : ' || total);
END;
--------------------------------------------------------------------------------
--활용
DECLARE
  v_empno emp.empno%TYPE;
  v_name  emp.ename%TYPE := UPPER('&name');
  v_sal   emp.sal%TYPE;
  v_job   emp.job%TYPE;
BEGIN
  select empno , job ,sal
    into v_empno, v_job , v_sal
  from emp
  where ename = v_name;

  IF v_job IN('MANAGER','ANALYST') THEN
     v_sal := v_sal * 1.5;
  ELSE
     v_sal := v_sal * 1.2;
  END IF;

  update emp
  set sal = v_sal
  where empno=v_empno;

  DBMS_OUTPUT.PUT_LINE(SQL%ROWCOUNT || '개의 행이 갱신 되었습니다');

  --예외처리
  EXCEPTION
    WHEN NO_DATA_FOUND THEN
       DBMS_OUTPUT.PUT_LINE(v_name || '는 자료가 없습니다');
    WHEN TOO_MANY_ROWS THEN
       DBMS_OUTPUT.PUT_LINE(v_name || '는 동명 이인입니다');
    WHEN OTHERS THEN
       DBMS_OUTPUT.PUT_LINE('기타 에러가 발생했습니다');
END;
/*
질의는 하나의 행만 RETURN 해야 합니다. PL/SQL 블록 내의 SELECT 문장은 다음 규칙을
적용하는 Embedded SQL 의 ANSI 범주에 속합니다. 질의의 결과는 하나의 행만을 RETURN 해
야  하고  하나의  행  이상  또는  행이  없는  것은  에러를  생성합니다.  PL/SQL 은
NO_DATA_FOUND 와 TOO_MANY_ROWS 를 예외로 블록의 예외 섹션에서 추적할 수 있는 표준 예
외를 조성하여 처리 합니다.
*/
select * from emp where ename='SMITH';
rollback;

-- pl-sql 기본 구문  END
--------------------------------------------------------------------------------
-- cursor , procedure , function , Trigger 고급자원

--[ 커서 ]
--지금까지 집합 단위의 데이터 처리 (전체 row를 대상으로)

--[CURSOR]
--1.  [행단위]로 데이터를 처리하는 방법을 제공
--2.  여러건의 데이터를 처리하는 처리하는 방법을 제공 (한 건이상의  row가지고 놀기)

--사원급여테이블(건설회사)
--정규직 , 일용일 ,시간직

--사번 , 이름 , 직종명 , 월급 , 시간 , 시간급 , 식대
-- 10   홍길동  정규직   120    null   null     null
-- 11   김유신  시간직   null   10      100     null
-- 12   이순신  일용일   null   null    120     10

--정규직
--월급
--
--일용직
--시간 , 시간급
--
--시간직
--시간급 , 식대


--한 행식씩 접근해서 직종을 기준으로 계산방법

--if 정규직  > 월급 (총급여)
--elsif 시간직 > 시간 * 시간급 (총급여)
--elsif 일용직 > 시간급 + 식대 (총급여)



--SQL CURSOR 의 속성을 사용하여 SQL 문장의 결과를 테스트할 수 있다.
--[종 류 설 명]
  --SQL%ROWCOUNT 가장 최근의 SQL 문장에 의해 영향을 받은 행의 수
  --SQL%FOUND 가장 최근의 SQL 문장이 하나 또는 그 이상의 행에 영향을 미친다면 TRUE 로 평가한다.
  --SQL%NOTFOUND 가장 최근의 SQL 문장이 어떤 행에도 영향을 미치지 않았다면 TRUE 로  평가한다.
  --SQL%ISOPEN PL/SQL 이 실행된 후에 즉시 암시적 커서를 닫기 때문에 항상 FALSE 로 평가된다.

/*
   DECLARE
          CURSOR 커서이름 IS 문자(커서가 실행할 쿼리)
   BEGIN
         OPEN 커서이름 (커서가 가지고 있는 쿼리를 실행)

         FETCH 커서이름 INTO 변수명들...
          --커서로 부터 데이터를 읽어서 원하는 변수에 저장
         CLOSE 커서이름 (커서닫기)
   END


*/
DECLARE
  vempno emp.empno%TYPE;
  vename emp.ename%TYPE;
  vsal   emp.sal%TYPE;
  CURSOR c1  IS select empno,ename,sal from emp where deptno=30;
BEGIN
    OPEN c1; --커서가 가지고 있는 문장 실행
    LOOP
      --Memory
      /*
        7499 ALLEN 1600
        7521 WARD 1250
        7654 MARTIN 1250
        7698 BLAKE 2850
        7844 TURNER 1500
        7900 JAMES 950
      */
      FETCH c1 INTO vempno , vename, vsal;
      EXIT WHEN c1%NOTFOUND; --더이상 row 가 없으면 탈출
        DBMS_OUTPUT.PUT_LINE(vempno || '-' || vename || '-'|| vsal);
    END LOOP;
    CLOSE c1;
END;
-------------------------------------------------------
--위 표현을 좀 더 간단하게
--java (for(emp e : emplist){}
DECLARE
  CURSOR emp_curr IS  select empno ,ename from emp;
BEGIN

    FOR emp_record IN emp_curr  --row 단위로 emp_record변수 할당
    LOOP
      EXIT WHEN  emp_curr%NOTFOUND;
      DBMS_OUTPUT.PUT_LINE(emp_record.empno || '-' || emp_record.ename);
    END LOOP;

    CLOSE emp_curr;
END;

--------------------------------------------------------------------------------
DECLARE
  vemp emp%ROWTYPE; --Type 정의
  CURSOR emp_curr IS  select empno ,ename from emp;
BEGIN
  FOR vemp IN emp_curr  --row 단위로 emp_record변수 할당
    LOOP
      EXIT WHEN  emp_curr%NOTFOUND;
      DBMS_OUTPUT.PUT_LINE(vemp.empno || '-' || vemp.ename);
    END LOOP;
    CLOSE emp_curr;
END;
-------------------------------------------------
DECLARE
  v_sal_total NUMBER(10,2) := 0;
  CURSOR emp_cursor
  IS SELECT empno,ename,sal FROM emp
     WHERE deptno = 20 AND job = 'CLERK'
     ORDER BY empno;
BEGIN
  DBMS_OUTPUT.PUT_LINE('사번 이 름 급 여');
  DBMS_OUTPUT.PUT_LINE('---- ---------- ----------------');
  FOR emp_record IN emp_cursor
  LOOP
      v_sal_total := v_sal_total + emp_record.sal;
      DBMS_OUTPUT.PUT_LINE(RPAD(emp_record.empno,6) || RPAD(emp_record.ename,12) ||
                           LPAD(TO_CHAR(emp_record.sal,'$99,999,990.00'),16));
  END LOOP;
      DBMS_OUTPUT.PUT_LINE('----------------------------------');
      DBMS_OUTPUT.PUT_LINE(RPAD(TO_CHAR(20),2) || '번 부서의 합 ' ||
      LPAD(TO_CHAR(v_sal_total,'$99,999,990.00'),16));
END;

--------------------------------------------------------------------------------
create table cursor_table
as
  select * from emp where 1=2;

select* from cursor_table;  

alter table cursor_table
add totalsum number;

--문제
--emp 테이블에서  사원들의  사번 , 이름 , 급여를 가지고
--와서 cursor_table insert 를 하는데 totalsum 은 급여 + comm 통해서
--부서번호가 10인 사원은 totalsum에 급여 정보만 넣으세요
--데이터 처리
--
insert into CURSOR_TABLE(empno,ename,sal,totalsum)
select empno , ename , sal , sal+nvl(comm,0)
from emp where deptno=20;

select *  from CURSOR_TABLE;

DECLARE
  result number := 0;
  CURSOR emp_curr IS select empno, ename, sal,deptno,comm from emp;
  BEGIN
    FOR vemp IN emp_curr   --row 단위로 emp_record 변수에 할당
    LOOP
        EXIT WHEN emp_curr%NOTFOUND;
        IF(vemp.deptno = 20) THEN
              result := vemp.sal+nvl(vemp.comm,0);
              insert into cursor_table(empno, ename, sal,deptno,comm,totalsum)
              values (vemp.empno,vemp.ename, vemp.sal,vemp.deptno,vemp.comm,result);
        ELSIF(vemp.deptno = 10) THEN
            result := vemp.sal;
              insert into cursor_table(empno, ename, sal,deptno,comm,totalsum)
              values (vemp.empno,vemp.ename, vemp.sal,vemp.deptno,vemp.comm,result);
        ELSE
            DBMS_OUTPUT.PUT_LINE('ETC');
        END IF;
     END LOOP;   
  END;

rollback;
commit;


select * from cursor_table order by deptno;


--PL-SQL 트랜잭션 및 예외 처리하기
 DECLARE
    v_ename emp.ename%TYPE := '&p_ename';
    v_err_code NUMBER;
    v_err_msg VARCHAR2(255);
    BEGIN
          DELETE emp WHERE ename = v_ename;
          IF SQL%NOTFOUND THEN
              RAISE_APPLICATION_ERROR(-20002,'my no data found'); --사용자가 예외 만들기
          END IF;
       EXCEPTION
        WHEN OTHERS THEN
            ROLLBACK;
              v_err_code := SQLCODE;
              v_err_msg := SQLERRM;
              DBMS_OUTPUT.PUT_LINE('에러 번호 : ' || TO_CHAR(v_err_code));
              DBMS_OUTPUT.PUT_LINE('에러 내용 : ' || v_err_msg);
      END;

select * from emp where ename ='KING';

delete from emp where ename='aaa';
--------------------------------------------------------------------------------
--지금까지 만들었는 작업이 영속적으로 저장 되지 않았다
--crerate table , create view
--내가 위에서 만든 커서를 영속적으로 저장 (객체)
--객체 형태로 저장 해놓으면 그 다음번에 코딩하지 않고 불러 사용

--Oracle : subprogram(procedure)
--Ms-sql : procedure

--자주 사용되는 쿼리를 모듈화 시켜서 객체로 저장하고
--필요한 시점에 불러(호출) 해서 사용하겠다

create or replace procedure usp_emplist
is
  BEGIN
    update emp
    set job = 'TTT'
    where deptno=30;
  END;

--실행방법
execute usp_emplist;
select * from emp where deptno=30;
rollback;

--procedure  장점
--기존 : APP(emp.java > select .... 구문)    ->네트워크 > DB연결 > selet... > DB에
--지금 : APP(emp.java > usp_emplist 구문)    ->네트워크 > DB연결 > usp_emplist > DB에

--1.장점 : 네트워크 트래픽 감소(시간 단축)
--2.장점 : 보안 (네트워크 상에서 ...보안 요소)해결


--procedure
--parameter  사용가능
--종류 : INPUT  , OUTPUT
create or replace procedure usp_update_emp
(vempno emp.empno%TYPE)
is
  BEGIN
    update emp
    set sal = 0
    where empno = vempno;
  END;
--실행방법
exec usp_update_emp(7788);

select * from emp where empno = 7788;
rollback;

--------------------------------------------------------------------------------
create or replace procedure usp_getemplist
(vempno emp.empno%TYPE)
is
  --내부에서 사용하는 변수
  vname emp.ename%TYPE;
  vsal  emp.sal%TYPE;
  BEGIN
      select ename, sal
        into vname , vsal
      from emp
      where empno=vempno;

      DBMS_OUTPUT.put_line('이름은 : ' || vname);
      DBMS_OUTPUT.put_line('급여는 : ' || vsal);
  END;

exec usp_getemplist(7902);

--------------------------------------------------------------------------------
-- procedure  는 parameter  종류 2가지
--1. input paramter : 사용시 반드시  입력          (IN : 생략하는 default)
--2. output parmater : 사용시 입력값을 받지 않아요 (OUT)
create or replace procedure app_get_emplist
(
  vempno IN emp.empno%TYPE,
  vename OUT emp.ename%TYPE,
  vsal   OUT emp.sal%TYPE
)
is
  BEGIN
    select ename, sal
      into vename , vsal
    from emp
    where empno=vempno;
  END;

--오라클 실행 테스트
DECLARE
  out_ename emp.ename%TYPE;
  out_sal   emp.sal%TYPE;
BEGIN
   app_get_emplist(7902,out_ename,out_sal);
   DBMS_OUTPUT.put_line('출력값 : ' || out_ename || '-' || out_sal);
END;
---------------------기본 procedure END-----------------------------------------
--[사용자 정의 함수]
--to_char() , sum() 오라클에서 제공
--사용자가 직접 필요한 함수를 만들어 사용가능
--사용방법은 다른 함수사용법과 동일
--사용자 정의 함수 paramter  정의 , return 값
create or replace function f_max_sal
(s_deptno emp.deptno%TYPE)
return number   -- public int f_max_sal(int deptno) {  return 10}
is
  max_sal emp.sal%TYPE;
BEGIN
      select max(sal)
        into max_sal
      from emp
      where deptno = s_deptno;
      return max_sal;
END;

---
select * from emp where sal = f_max_sal(10);

select max(sal) , f_max_sal(30) from emp;
--
create or replace function f_callname
(vempno emp.empno%TYPE)
return varchar2 -- public String f_callname() { return "홍길동"}
is
  v_name emp.ename%TYPE;
BEGIN
    select ename || '님'
      into v_name
    from emp
    where empno=vempno;
    return v_name;
END;

select f_callname(7788) from dual;

select empno, ename , f_callname(7788) , sal
from emp
where empno=7788;

select empno, ename , f_callname(empno) , sal
from emp
where empno=7788;

--함수
--parmater  사번을 입력받아서 사번에 해당되는 부서이름을 리턴하는 함수
create or replace function f_get_dname
(vempno emp.empno%TYPE)
return varchar2
is
    v_dname dept.dname%TYPE;
  BEGIN
    select dname
      into v_dname
    from dept
    where deptno = (select deptno from emp where empno=vempno);
    return v_dname;
  END;

select empno , ename , f_get_dname(empno)
from emp
where empno=7788;
--------------------------function END------------------------------------------
--[트리거 : Trigger]
--트리거(trigger)의 사전적인 의미는 방아쇠나 (방아쇠를) 쏘다, 발사하다,
--(사건을) 유발시키다라는 의미가 있다.

--[입고]    [재고]     [출고]

--입고 INSERT (내부적으로 [트랜잭션]이 동작)
--재고 INSERT
--위험부담 : lock


--PL/SQL에서의 트리거 역시 방아쇠가 당겨지면 자동으로 총알이 발사되듯이
--어떠한 이벤트가 발생하면 그에 따라 다른 작업이 자동으로 처리되는 것을 의미한다.
/*
트리거란 특정 테이블의 데이터에 변경이 가해졌을 때 자동으로 수행되는
[저장 프로시저]라고 할 수 있다.
앞서 배운 저장 프로시저는 필요할 때마다 사용자가 직접
 EXECUTE 명령어로 호출해야 했다.
하지만 트리거는 이와 달리 테이블의
데이터가 INSERT, UPDATE, DELETE 문에 의해 변경되어질 때
[ 자동으로 수행되므로 이 기능을 이용하며 여러 가지 작업 ] 을 할 수 있다.
이런 이유로 트리거를 사용자가 직접 실행시킬 수는 없다.


--BEFORE : 테이블에서 DML 실행되기 전에 트리거가 동작
--AFTER :  테이블에서 DML 실행후에 트리거 동작

Syntax
CREATE [OR REPLACE] TRIGGER trigger_name
{BEFORE | AFTER} triggering_event [OF column1, . . .] ON table_name
[FOR EACH ROW [WHEN trigger_condition]
trigger_body;

trigger_name TRIGGER 의 식별자
  BEFORE | AFTER DML 문장이 실행되기 전에 TRIGGER 를 실행할 것인지 실행된
  후에 TRIGGER 를 실행할 것인지를 정의
triggering_event
TRIGGER 를 실행하는 DML(INSERT,UPDATE,DELETE)문을 기술한다.

OF column TRIGGER 가 실행되는 테이블에서 COLUMN 명을 기술한다.

table_name TRIGGER 가 실행되는 테이블 이름

FOR EACH ROW 이 옵션을 사용하면
행 레벨 트리거가 되어 triggering 문장
에 의해 영향받은 행에 대해 각각 한번씩 실행하고 사용하지
않으면 문장 레벨 트리거가 되어 DML 문장 당 한번만 실행된다.


  TRIGGER 에서 OLD 와 NEW
    행 레벨 TRIGGER 에서만 사용할 수 있는 예약어로 트리거 내에서 현재 처리되고 있는 행
    을 액세스할 수 있다. 즉 두개의 의사 레코드를 통하여 이 작업을 수행할 수 있다. :OLD
    는 INSERT 문에 의해 정의되지 않고 :NEW 는 DELETE 에 대해 정의되지 않는다. 그러나
    UPDATE 는 :OLD 와 :NEW 를 모두 정의한다. 아래의 표는 OLD 와 NEW 값을 정의한 표이다.
    문장 :OLD :NEW
    INSERT 모든 필드는 NULL 로 정의 문장이 완전할 때 삽입된 새로운 값
    UPDATE 갱신하기 전의 원래 값 문장이 완전할 때 갱신된 새로운 값
    DELETE 행이 삭제되기 전의 원래 값 모든 필드는 NULL 이다.

DROP TRIGGER 명령어로 트리거를 삭제할 수 있고 TRIGGER 를 잠시 disable 할 수 있다.
DROP TRIGGER trigger_name;
ALTER TRIGGER trigger_name {DISABLE | ENABLE};
TRIGGER 와 DATA DICTIONARY
TRIGGER 가 생성될 때 소스 코드는 데이터 사전 VIEW 인 user_triggers 에 저장된다. 이
VIEW 는 TRIGGER_BODY, WHERE 절, 트리거링 테이블, TRIGGER 타입을 포함 한다.

*/
create table tri_emp
as
  select empno , ename from emp where 1=2;


select * from tri_emp;

create or replace trigger tri_01
after insert on tri_emp
BEGIN -- 자동 동작할 내용
    DBMS_OUTPUT.PUT_LINE('신입사원 입사');
END;

create or replace trigger tri_02
after update on tri_emp
BEGIN
  DBMS_OUTPUT.PUT_LINE('신입사원 수정');
END;

--테이블에 trigger 정보
select * from user_triggers;


insert into tri_emp values(100,'김유신');

update tri_emp
set ename='아하'
where empno=100;


--delete 트리거 : tri_emp
--사원테이블 삭제 (화면 출력)
create or replace trigger tri_03
after delete on tri_emp
BEGIN
  DBMS_OUTPUT.PUT_LINE('신입사원 삭제');
END;

insert into tri_emp values(200,'홍길동');
update tri_emp set ename='변경' where empno= 200;
delete from tri_emp where empno=200;

--------------------------------------------------------------------------------
--트리거의 활용
create table tri_order
(
  no number,
  ord_code varchar2(10),
  ord_date date
);

--before 트리거의 동작시점이 실제 tri_order 테이블 insert 되기 전에
--트리거 먼저 동작 그 이후 insert 작업
create or replace trigger trigger_order
before insert on tri_order
BEGIN
  IF(to_char(sysdate,'HH24:MM') not between '09:00' and '16:00') THEN
     RAISE_APPLICATION_ERROR(-20002, '허용시간 오류 쉬세요');
  END IF;
END;

insert into tri_order values(1,'notebook',sysdate);
select * from tri_order;
commit;

--트리거 삭제
drop trigger trigger_order;

--POINT
--PL_SQL 두개의 가상데이터(테이블) 제공
--:OLD > 트리거가 처리한 레코드의 원래 값을 저장 (ms-sql (deleted)
--:NEW > 새값을 포함                             (ms-sql (inserted)

create or replace trigger tri_order2
before insert on tri_order
for each row
BEGIN
  IF(:NEW.ord_code) not in('desktop') THEN
     RAISE_APPLICATION_ERROR(-20002, '제품코드 오류');
  END IF;
END;

select * from tri_order;

--오류 (desktop)
insert into tri_order values(200,'notebook',sysdate);

insert into tri_order values(200,'desktop',sysdate);

select * from tri_order;
commit;

--------------------------------------------------------------------------------
--입고 , 재고

create table t_01 --입고
(
  no number,
  pname varchar2(20)
);

create table t_02 --재고
(
  no number,
  pname varchar2(20)
);

--입고 데이터 들어오면 같은 데이터를 재고 입력
create or replace trigger insert_t_01
after insert on t_01
for each row
BEGIN
  insert into t_02(no, pname)
  values(:NEW.no ,:NEW.pname);
END;

--입고
insert into t_01 values(1,'notebook');

select * from t_01;
select * from t_02;

-- 입고 제품이 변경 (재고 변경)
create or replace trigger update_t_01
after update on t_01
for each row
BEGIN
  update t_02
  set pname = :NEW.pname
  where no = :OLD.no;
END;

update t_01
set pname = 'notebook2'
where no = 1;

select * from t_01;

select * from t_02;

--delete 트리거 만들어 보세요
--입고 데이터 delete from t_01 where no =1 삭제 되면 재고 삭제
create or replace trigger delete_tri_01
after delete on t_01
for each row
BEGIN
  delete from t_02
  where no=:OLD.no;
END;

delete from t_01 where no=1;

select* from t_01;
select* from t_02;

commit;
```
