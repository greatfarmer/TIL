# Oracle Basic

```sql
/*
[1일차 수업]
1. 오라클 소프트웨어 다운로드
>> http://www.oracle.com/technetwork/database/database-technologies/express-edition/downloads/index.html

2. 11g express 버전 (무료버전) 설치

3. 설치 (관리자 권한: SYSTEM, SYS 계정: 암호설정 >> 1004)

4. sqlplus 기본 프로그램 접속확인

5. SqlDeveloper 무료툴을 설치
>> 유료툴 > 토드, 오렌지, sqlgate

6. Tool을 통해서 Oracle 접속
   >> HR계정 암호 1004 > unlock
   >> BITUSER, 1004 >> 계정 생성 (*접속자는 대소문자 구분 안함)

-- USER SQL
CREATE USER bituser IDENTIFIED BY 1004
DEFAULT TABLESPACE "USERS"
TEMPORARY TABLESPACE "TEMP";

-- QUOTAS
ALTER USER bituser QUOTA UNLIMITED ON USERS;

-- ROLES
GRANT "CONNECT" TO bituser ;
GRANT "RESOURCE" TO bituser ;

[실습테이블 구성 script]
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

*/
/*
SELECT [DISTINCT] {*, column [alias], ...}
FROM table_name [WHERE condition]
[ORDER BY {column, expression} [ASC | DESC]];
*/
--1. 사원테이블에서 모든 데이터를 출력하세요
select * from emp;
SELECT * FROM EMP; --쿼리문은 대소문자 구별하지 않아요

--2. 특정 컬럼 데이터 추출하기
select empno, ename, job, sal from emp;

select empno from emp;

select hiredate, sal, comm from emp;

--3. 컬럼에 가명칭(alias) 부여하기
select empno 사번, ename 이름
from emp;

select empno "사   번", ename "이   름" --쌍따옴표는 alias에서만 사용
from emp;

--정식(ansi 문법) >> 권장
select empno as "사   번", ename as "이   름"
from emp;

--Oracle: 문자열 (문자 데이터 엄격하게 대소문자 구분)
--소문자; a, 대문자: A 를 다른 문자 취급
select * from emp where enmae = 'king';
select * from emp where ename = 'KING';
select * from emp where ename = 'King';

--Oracle: 연산자 (결합 연산자) > || >> 'hello' || 'world' >> 'helloworld'
--Java: + 연산자 (숫자 + 숫자 = 연산)
--Java: + 연산자 (문자 + 문자 = 결합)
--TIP) ms-sql: + 연산, 결합 모두 실시함

select '사원의 이름은 ' || ename || '입니다' as "ename"
from emp;

--Point 테이블 가지는 Column의 기본 타입
--컬럼이 숫자 타입일까? 문자 타입일까?
desc emp; --가장 기본적인

select empno || ename as "결합" --내부적으로 자동 형변환 (숫자 -> 문자) to_char()
from emp;

select empno + ename as "결합" --실제 연산 작업 Error > "invalid number"
from emp;

--사장님 우리회사에 직종이 몇개나 있나
--중복 데이터 제거: 키워드 (distinct)
select DISTINCT job from emp;

select distinct deptno from emp;

--distinct의 원리 (컬럼이 1개 이상)...grouping
select distinct job, deptno from emp order by job;

select distinct deptno, job from emp order by deptno;
----------------------------------------------------------
--오라클 언어(SQL)
--Java 같은 언어다 (연산자)
--Java 거의 동일 (Java: % 나머지) >> 오라클 % 검색 패턴으로 사용 >> 별도의 함수 나머지 Mod() 함수
--산술(+, -, *, / ...) Mod() 함수

--사원테이블에서 사원의 급여를 100달러 인상한 결과를 출력하세요
--1. 컬럼의 타입 (Type) >> number
desc emp;
select empno, ename, sal, sal + 100 as "인상급여"
from emp;

--^^
select 100 + 100 from dual; -- 데이터 TEST 임시 dual
select 100 || 100 from dual; -- 숫자 문자로 자동 형변환
select '100' + 100 from dual; --문자를 숫자로 형변환
select 'A100' + 100 from dual; --Error

--비교연산자
-- >, <, <= ...
--Java 같다(==), 할당(=)
--Oracle 같다(=), 같지않다(!=)

--논리 연산자
--AND, OR, NOT

/*
SELECT [DISTINCT] {*, column [alias], ...}
FROM table_name [WHERE condition]
[ORDER BY {column, expression} [ASC | DESC]];
*/

--조건절 (원하는 row 가지고 오겠다)
select *
from emp
where sal >= 3000;

select empno, sal
from emp
where sal >= 3000;

--이상, 이하 (= 포함)
--초과, 미만

--사번이 7788번인 사원의 사번, 이름, 직종, 입사일을 출력
select empno, ename, job, hiredate
from emp
where empno = 7788;

--사원의 이름이 KING인 사원의 사번, 이름, 급여를 출력
select empno, ename, sal
from emp
where ename = 'KING'; --대소문자 엄격하게 구분 (varchar2(20)...)

--논리 (AND, OR, NOT)
--급여가 2000달러 이상이면서 직종이 manager인 사원의 모든 정보를 출력
select *
from emp
where sal >= 2000 and job = 'MANAGER';

--급여가 2000달러 이상이거나 직종이 manager인 사원의 모든 정보를 출력
select *
from emp
where sal >= 2000 or job = 'MANAGER';

--튜닝 기본 (실행순서)
--select절 3
--from절   1
--where절 2
-------------------------------------------------------------
--오라클 날짜
--서버의 날짜
--시스템 (웹사이트) 제작: 날짜 >> sysdate
--회원가입: insert into board(writer, title, content, regdate)
--         values('홍길동', '방가방가', '피곤해요', sysdate);
--TIP) ms-sql >> select getdate() ...
select sysdate from dual;

select hiredate from emp;
desc emp;

--오라클 시스템 정보를 담고 있는 테이블
select * from SYS.NLS_SESSION_PARAMETERS; --환경설정 정보
--7 NLS_DATE_FORMAT -> RR/MM/DD
--8 NLS_DATE_LANGUAGE -> KOREAN
--10 NLS_TIME_FORMAT ->	HH24:MI:SSXFF

select * from SYS.NLS_SESSION_PARAMETERS where parameter = 'NLS_DATE_FORMAT';

--현재 접속한 사용자(session) 기준으로 적용
--현재 접속을 해제하고 다시 접속하면 >> 원상태로 복구 NLS_DATE_FORMAT -> RR/MM/DD
--다른 곳에서 bituser로 다른 사용자가 접속하면 NLS_DATE_FORMAT -> RR/MM/DD
alter session set nls_date_format='YYYY-MM-DD HH24:MI:SS'

select sysdate from dual;
select hiredate from emp;

--오라클 날짜 표기: '날짜'
select * from emp
where hiredate='1980-12-17'; --날짜 형식으로 인정

select * from emp
where hiredate='1980/12/17'; --날짜 형식으로 인정

select * from emp
where hiredate='80/12/17'; --인정 안되요: YYYY
--RR-MM-DD

--사원의 급여가 2000이상이고 4000이하인 모든 사원의 정보를 출력
select *
from emp
where sal >= 2000 and sal <= 4000;

--(외우기!)연산자: 컬럼명 between A and B (=을 포함)
select *
from emp
where sal between 2000 and 4000;

--사원의 급여가 2000초과이고 4000미만인 모든 사원의 정보를 출력
--between 사용할 수 없음
select *
from emp
where sal > 2000 and sal < 4000;

--부서 번호가 10번 또는 20번 또는 30번인 사원의 사번, 이름, 급여, 부서번호를 출력
select empno, ename, sal, deptno
from emp
where deptno=10 or deptno=20 or deptno=30;

--IN 연산자(조건 or 조건 or...): 컬럼 하나만 됨
select empno, ename, sal, deptno
from emp
where deptno in (10, 20, 30);

--부서 번호가 10번 또는 20번 또는 30번이 아닌 사원의 사번, 이름, 급여, 부서번호를 출력
--부정 !=
select empno, ename, sal, deptno
from emp
where deptno !=10 and deptno !=20;

--NOT IN 연산자 (and 부정값 and 부정값)
select empno, ename, sal, deptno
from emp
where deptno not in (10, 20);

--POINT: 값이 없다(데이터 없다) >> null
--필요악 >> null
create table member(
 userid varchar2(20) not null,
 name varchar2(20) not null,
 hobby varchar2(20) --default null(null값을 허용하겠다): 필수입력 사항이 아니다
);

select * from member;
insert into member(userid, hobby) values('hong', '농구');
--SQL 오류: ORA-01400: cannot insert NULL into ("BIT_USER"."MEMBER"."NAME")

insert into member(userid, name) values('hong', '홍길동'); --hobby 값을 넣지 않으면 null
insert into member(userid, name, hobby) values('hong', '홍길동', '야구');
commit; --실제 반영(물리적인 디스크에 쓰는 작업)

/*
delete from emp;
select * from emp;
rollback; 롤백하는 법
*/

--수당을 받지 않는 모든 사원의 정보를 출력
select * from emp where comm = null; --존재하지 않아요 (x)

select * from emp where comm is null; --comm 컬럼 null값을 가지고 있는 데이터

--수당을 받는 모든 사원의 정보를 출력 (null이 아닌 사원)
select * from emp where comm is not null;

--사원테이블에서 사번, 이름, 급여, 수당, 총급여를 출력하세요
--총급여(급여+수당)
select empno, ename, sal, comm, sal + comm as "총급여"
from emp;

--(외우기!)
--POINT null
--null과의 모든 연산은 그 결과가: null
--위 문제를 해결 null처리 함수: nvl(), nvl2() >> 함수중에 가장 중요 nvl(NULL by a Value Like in)
--ms-sql: convert()
--my-sql: IFNULL()

select empno, ename, sal, comm, sal + nvl(comm, 0) as "총급여"
from emp;

select 1000 + null from dual;

select 1000 + nvl(null, 0) from dual;

select comm, nvl(comm, 0) from emp;

--사원의 급여 1000 이상이고 수당을 받지 않는 사원의 사번, 이름, 직종, 급여, 수당을 출력
select empno, ename, job, sal, comm
from emp
where sal >= 1000 and comm is null;
--------------------------------------------------------------------------------
--Query: 질의
--DQL(data query language): SELECT
--DDL(data definition language, 데이터 정의어): create, alter, drop: 객체생성, 수정, 삭제

--class Borad{private int boardid;} 와 유사한 의미

--create table은 commit, rollback 쓰지 않고 바로 디스크에 저장
create table board(
  boardid number not null, --필수 입력 (숫자)
  title varchar2(20) not null, --한글 한자 2byte >> 10자, 영문자 특수 공백 >> 20자
  content varchar2(2000) not null,
  hp varchar2(20)
);

desc board;

--insert, update, delete 작업시에는 (rollback, commit) 반드시 수행
insert into board(boardid, title, content)
values(100, '오라클', '참 쉽네');

select * from board;
--실반영: commit
--취소: rollback
commit;

insert into board(boardid, title, content)
values(200, '자바', '그립다...');

select * from board;
--잘못되었는데
rollback;

select * from board;

insert into board(boardid, title, content, hp)
values(200, '자바', '그립다...', '010...');

select * from board;
commit;

select boardid, nvl(hp, 'EMPTY') as "hp" from board; -- " "는 alias에 사용
--nvl() 함수는 숫자, 날짜, 문자 모두 적용가능하다
------------------------------------------------------
--문자열 검색
--주소검색: '역삼' 검색하면 역삼이란 글자가 들어있는 모두 문장
--문자열 패턴 검색 (like 연산자)

--like 연산자 (와일드 카드 문자(%: 모든 것, _: 한문자) 결합...

select *
from emp
where ename like '%A%'; --A가 들어있는 ename 모두 검색

select *
from emp
where ename like 'A%'; --A로 시작하는 ename 모두 검색, 대소문자 구별

select *
from emp
where ename like '김%'; --한글 가능

select *
from emp
where ename like '%S'; --S로 끝나는 모든 ename 검색

select *
from emp
where ename like '%LL%'; --LL이 들어가는 모든 ename 검색

select *
from emp
where ename like '%A%A%'; --A 글자 2개

select ename
from emp
where ename like '_A%'; --_한문자(_는 찾고자하는 문자의 순서를 정함) >> 두번째 단어가 A로 시작

--오라클 과제 (regexp_like): 상세한 표현
--select * from emp where REGEXP_LIKE (ename, '[A-C]');
--정규 표현식(표준: java, oracle, script ...)
--조별 스터디(중간 프로젝트시: 검증(정규표현식쓰는 것 권장)
-------------------------------------------------------------------------------------
--데이터 정렬하기
--order by 컬럼명: 문자, 숫자, 날짜
--오름차순: asc: 낮은순 (default)
--내림차순: desc: 높은순

select * from emp order by sal; --급여가 낮은 순으로 asc
select * from emp order by sal asc;

--급여를 많이 받는 순으로 정렬해서 출력
select * from emp order by sal desc;

select ename from emp order by ename asc; --문자열 적용가능

--입사일이 가장 늦은 순으로 정렬해서 사번, 이름, 급여, 입사일 출력
select empno, ename, sal, hiredate
from emp
order by hiredate desc;

/*
실행순서
select     3
from       1
wehre      2
order by   4 (select 결과를 정렬)
*/

--문제 만들기: 직종이 MANAGER인 직원 중에서 입사일이 가장 늦은 순으로 정렬해서 사번, 이름, 급여, 직종, 입사일 순 출력
select empno, ename, sal, job, hiredate
from emp
where job='MANAGER'
order by hiredate desc;

--문제
select deptno, job
from emp
order by deptno desc, job asc;
--DEPTNO순으로 정렬하고, DEPTNO가 같은 것 끼리 그룹하고, 그룹내에서 job의 오름차순으로 정렬함
--grouping
-----------------------------------------------------------------------------------
--연산자
--합집합(union): 테이블과 테이블의 데이터 합치는 것(중복값은 배제)
--합집합(union all): 중복값 허용

create table UTA (name varchar2(20));
insert into uta(name) values('AAA');
insert into uta(name) values('BBB');
insert into uta(name) values('CCC');
insert into uta(name) values('DDD');
commit;
select * from uta;

create table UT(name varchar2(20));
insert into ut(name) values('AAA');
insert into ut(name) values('BBB');
insert into ut(name) values('CCC');
commit;
select * from ut;

select * from ut
union -- 중복값 배제
select * from uta;

select * from ut
union all -- 중복값 포함
select * from uta;

--union 규칙
--1. 대응되는 컬럼의 타입이 동일
select empno, ename from emp
union
select dname, deptno from dept; --(x)

select empno, ename from emp
union
select deptno, dname from dept;

--실무 > subquery 사용해서 union한 테이블을 가상 테이블처럼 사용
select empno, ename
from (
  select empno, ename from emp
  union
  select deptno, dname from dept
)
order by empno desc;

--2. 대응되는 컬럼의 개수가 동일 (null 착한일)
select empno, ename, job, sal from emp
union
select deptno, dname, loc, null from dept;
--여기까지 초보 개발자가 의무적으로 (단일 테이블) 대상으로
--------------------------------------------------------------
--오라클 함수 (오라클.pdf 49page)
/*
단일 행 함수의 종류
1) 문자형 함수 : 문자를 입력 받고 문자와 숫자 값 모두를 RETURN 할 수 있다.
2) 숫자형 함수 : 숫자를 입력 받고 숫자를 RETURN 한다.
3) 날짜형 함수 : 날짜형에 대해 수행하고 숫자를 RETURN 하는 MONTHS_BETWEEN 함수를 제외하고 모두 날짜 데이터형의 값을 RETURN 한다.
4) 변환형 함수 : 어떤 데이터형의 값을 다른 데이터형으로 변환한다. (to_char(), to_number(), to_date())
5) 일반적인 함수 : NVL(), DECODE()
*/

--문자열 함수
select initcap('the super man') from dual; --The Super Man

select lower('AAA'), upper('aaa') from dual;

select ename, lower(ename) as "ename" from emp;

select * from emp where lower(ename) = 'king';

--문자의 개수 (length)
select length('abcd') from dual;
select length('홍길동') from dual; --byte수가 아님 문자 개수
select length(' 홍a길 동') from dual;

--결합 연산자 ||
--concat()
select 'a' || 'b' from dual;

select 'a' || 'b' || 'c' from dual; --확장성은 결합연산자가 좋음

select concat('a', 'b') from dual;

select concat(ename, job) from emp;
select ename || ' ' || job from emp; --확장성은 결합연산자가 좋음

--부분 문자열 추출
--java (substring)
--oracle (substr)
select substr('ABCDE', 2, 3) from dual; --BCD
select substr('ABCDE', 1, 1) from dual; --자기자신: A
select substr('ABCDE', 3, 1) from dual;

select substr('ABCDE', 3) from dual;
select substr('ABCDE', -2, 1) from dual; -- -는 뒤에서부터

select substr('ABCDE', -2, 2) from dual; --DE

--Quiz
/*
사원테이블에서 ename 컬럼 데이터에 대해서첫글자는 소문자로
나머지 문자는 대문자로 출력하되 하나의 컬럼 데이터로 출력하세요
--컬럼의 가명칭: fullname
--첫글자와 나머지 문자 사이에 공백하나 있어요
--ex) SMITH -> s MITH
*/
select lower(substr(ename, 1, 1)) from emp;
select upper(substr(ename, 2)) from emp;

select lower(substr(ename, 1, 1)) || ' ' || upper(substr(ename, 2)) as "fullname"
from emp;

--lpad, rpad(채우기)
select lpad('ABC', 10, '*') from dual;
select rpad('ABC', 10, '#') from dual;

--Quiz
--사용자 비번: hong1006
--화면: ho****** 출력하고 싶어요 (만약에 1004 >> 10**)
select rpad(substr('hong1006', 1, 2), length('hong1006'), '*') as "password" from dual;
select rpad(substr('1004', 1, 2), length('1004'), '*') as "password" from dual;

--emp 테이블에서 ename 컬럼의 데이터 출력하는데 첫글자만 출력하고 나머지는 * 출력하세요
select rpad(substr(ename, 1, 1), length(ename), '*') as "ename" from emp;

--SQL에서 가공해서 Java에서 쓸 것인지, SQL에서 보내고 Java에서 가공할 것인지 선택해야함 (사람마다 다름)

create table member2 (
  id number,
  jumin varchar2(14)
);

select * from member2;
insert into member2(id, jumin) values(100, '123456-1234567');
insert into member2(id, jumin) values(200, '234567-1234567');
commit;

--Quiz
--출력결과
--하나의 컬럼 데이터
--100: 123456-*******
--200: 234567-******* 출력
--가명칭 "jumin"

select id || ': ' || rpad(substr(jumin, 1, 7), length(jumin), '*') as "jumin" from member2;

--rtrim 함수
--[오른쪽 문자]지워라
select rtrim('MILLER', 'ER') from dual;

--ltrim
select ltrim('MILLER', 'MIL') from dual; -- 'L'은 중복되어서 같이 지움, ltrim('LMILLER', 'MIL')이어도 결과는 'ER'

--치환함수
select ename, replace(ename, 'A', '와우') from emp;
-------------------------------[문자열 함수 END]-------------------------------------
--[숫자함수]
--round (반올림 함수)
--trunc (절삭함수)
--나머지 구하는 함수 mod()

--round (반올림 함수)
--  -3 -2 -1 0(정수) 1 2 3
select round(12.345, 0) as "r" from dual; --정수부만 남겨라
select round(12.567, 0) as "r" from dual; --13 (반올림)

select round(12.345, 1) as "r" from dual; --12.3
select round(12.567, 1) as "r" from dual; --12.6

select round(12.567, -1) as "r" from dual; --10
select round(15.345, -1) as "r" from dual; --20
select round(15.345, -2) as "r" from dual; --0

--trunc (절삭함수: 반올림 하지 않고 버림)
select trunc(12.345, 0) as "r" from dual; --12정수부만 남겨라
select trunc(12.567, 0) as "r" from dual; --12 (반올림)

select trunc(12.345, 1) as "r" from dual; --12.3
select trunc(12.567, 1) as "r" from dual; --12.5

select trunc(12.567, -1) as "r" from dual; --10
select trunc(15.345, -1) as "r" from dual; --10
select trunc(15.345, -2) as "r" from dual; --0

select trunc(12.789789789789789789789789, 1) from dual;
select round(12.789789789789789789789789, 1) from dual;

--나머지
select 12/10 from dual; --몫, 나머지 다 나옴
select mod(12, 10) from dual; --나머지 2값
select mod(0, 0) from dual;
----------------------[숫자 함수 END]---------------------------
--날짜 함수: sysdate
--날짜 연산(POINT)
--Date + Number >> Date
--Date - Number >> Date
--Date - Date >> Number

select * from SYS.NLS_SESSION_PARAMETERS where parameter = 'NLS_DATE_FORMAT';

select sysdate from dual;
select hiredate from emp;

select MONTHS_BETWEEN('2018-02-27', '2010-02-27') from dual;

select MONTHS_BETWEEN(sysdate, '2010-01-01') from dual;
select round(MONTHS_BETWEEN(sysdate, '2010-01-01'), 1) from dual;
select trunc(MONTHS_BETWEEN(sysdate, '2010-01-01'), 1) from dual;

select to_date('2015-04-25') + 1000 from dual; --문자 -> 날짜 >> to_date()

select sysdate + 100 from dual;

--quiz
/*
1.사원테이블에서 사원들의 입사일에서 현재날짜(sysdate) 까지의 근속 월수를 구하세요
--단, 근속월수는 정수부분만 남기세요

2.한달이 31일이라고 가정하고 근속월수를 구하세요
*/
select hiredate, sysdate, trunc(MONTHS_BETWEEN(sysdate, hiredate), 0) as "month",
trunc((sysdate - hiredate) / 31, 0) as "31days-month"
from emp;

-------------------------------[날짜 함수 END]--------------------------------------
--문자
--숫자
--날짜
-----------------------------------------------------------------------------------
--[변환함수]POINT
--Oracle: 문자, 숫자, 날짜
--to_char(): 숫자 -> 문자 ($100,000), 날짜 -> 문자(2018년 12월 12일) (형식정의) 60%
--to_date(): 문자 -> 날짜 >> select '2018-12-12' + 1000 >> select to date('2018-12-12')..
--to_number(): 문자 -> 숫자 (자동 형변환: 껍질을 벗기기 때문에)

select '100' + 100 from dual; --자동 형변환되므로 활용도가 높음
select to_number('100') + 100 from dual; --원칙적인 방법, 하지만 자동 형변환되기 때문에 활용도가 떨어짐

/*
오라클 기본 타입(데이터 타입)
create table 테이블명 (컬럼명 타입);
create table memeber (age number); 100건 데이터 insert
--java > class member {int age} >> member m = new member(); 1건
--java > List<member> list = new ArrayList<>(); list.add(new member()); 여러 건

문자 타입
--char(20) >> 20byte >> 한글10자, 영문자, 특수문자, 공백 20자 >> 고정길이문자열
--varchar2(20) >> 20byte >> 한글10자, 영문자, 특수문자, 공백 20자 >> 가변길이문자열

char(20) >> '홍길동' >> 20byte 모두 사용
varchar2(20) >> '홍길동' >> 공간의 크기 >> 6byte

고정된데이터: 남, 여 >> 처리 >> char(2)
결국 >> varchar2(2)

성능 상의 문제))
char() .....varchar2() 보다 검색상 우선...
따라서, 고정된 데이터는 char()쓰는 것이 varchar2() 쓰는 것보다 성능 상 좋다.

결국 문제는 한글
unicode (2byte): 한글, 영문자, 특수문자, 공백 >> 2byte
nchar(20) >> 20은 문자의 개수 >> 실제 byte * 2 >> 40Byte
nvarchar2(20) >> 20개의 문자

한글 20자, 영문자 20자

*/
-----------------------------------------------------------------------
--1. to_number(): 문자를 숫자로
select 1 + 1 from dual;

select '1' + 1 from dual; --자동 형변환

select to_number('1') + 1 from dual; --원칙적인 방법

select '1' + '1' from dual; -- 2

select '1A' + '1' from dual; -- error

--2. to_char(): 숫자 -> 문자(형식문자), 날짜 -> 문자(형식문자)
select sysdate from dual;
--YYYY YY MM DD ... 정의되어 있다

select sysdate || '일' from dual; --자동 형변환 (날짜와 문자가 결합 >> 문자)
--원칙
select to_char(sysdate) || '일' from dual;

select sysdate,
to_char(sysdate, 'YYYY') || '년' as "YYYY",
to_char(sysdate, 'YEAR'),
to_char(sysdate, 'MM'),
to_char(sysdate, 'DD'),
to_char(sysdate, 'DAY'),
to_char(sysdate, 'DY')
from dual;

--Quiz
--입사일이 12월인 사원들의 사번, 이름, 입사일, 입사년도, 입사월을 출력하세요
select empno as "사번", ename as "이름", hiredate as "입사일", to_char(hiredate, 'YYYY') as "입사년도", to_char(hiredate, 'MM') as "입사월"
from emp
where to_char(hiredate, 'MM') = '12';

select to_char(hiredate, 'YYYY MM DD') as "일자" from emp;

select to_char(hiredate, 'YYYY"년" MM"월" DD"일"') as "일자" from emp;
--------------------------------------------------------------------------------------------------
--to_char(): 숫자 -> 문자
--why: 1000000000 -> $100,000,000 이런 형식의 문자데이터 to_char
select '>' || to_char(12345, '999999999999999') || '<' from dual;
select '>' || to_char(12345, '099999999999999') || '<' from dual;
select '>' || to_char(12345, '000000000000000') || '<' from dual;

select '>' || to_char(12345, '$999,999,999,999,999') || '<' from dual;

select to_char(sal, '$999,999') as "sal"
from emp;
--------------------------------------------------------------------------
--to_date(): 문자 -> 날짜
select to_date('2018-12-12', 'YYYY-MM-DD') from dual; --2018-12-12 00:00:00

select to_date('2018-12-12') + 100 from dual;

select '2018-12-12' + 100 from dual; --error
--------------------[변환 함수: to_number(), to_char(), to_date()]----------------
--문자
--숫자
--날짜
--변환(to_char ...)
---------------------------------------------------------------------------------
--[일반함수]
--프로그램적인 성향이 강하다..
--nvl(), nvl2() --> null 처리 함수
--decode() 함수 --> java의 if
--case() 함수 --> java의 switch

select comm, nvl(comm, 0) from emp;

create table t_emp(
  id number(6),
  job varchar2(20)
);

insert into t_emp(id, job) values(100, 'IT');
insert into t_emp(id, job) values(200, 'SALES');
insert into t_emp(id, job) values(300, 'MGR');
insert into t_emp(id) values(400);
insert into t_emp(id, job) values(500, 'MGR');
commit;

select * from t_emp;

--1. nvl()
select id, job, nvl(job, 'Empty...') from t_emp;

--2. nvl2() -> nvl2(데이터, 'null이 아닌 경우', 'null인 경우')
select id, job, nvl2(job, 'AAA', 'BBB') from t_emp; --job null이 아니면 앞의 것, null이면 뒤의 것

select id, job, nvl2(job, job || '입니다', 'empty') from t_emp;

--3. decode POINT(일반 sql 제어문이 없다 if, switch)
--decode(표현식, 조건1, 결과1, 조건2, 결과2, 조건3, 결과3 ...)
--통계데이터 추출
select id, job, decode(id, 100, 'IT...',
                           200, 'SALES...',
                           300, 'MGR...',
                           'ETC...') as "decodejob"
from t_emp;

select job, decode(job, 'IT', 1) from t_emp;
--활용(통계)
select count(decode(job, 'IT', 1)) as "job",
count(decode(job, 'SALES', 1)) as "SALES",
count(decode(job, 'MGR', 1)) as "MGR"
from t_emp;

select count(empno) from emp;
------------------------------------------------------------
/*
emp 테이블에서 부서번호가 10이면 '인사부', 20 '관리부', 30 '회계부' 나머지 부서 번호는
'기타부서'라고 출력하는 쿼리문을 만드세요
decode를 사용해서
*/
select empno, ename, deptno, decode(deptno, '10', '인사부',
                                            '20', '관리부',
                                            '30', '관리부',
                                            '기타부서') as "부서"
from emp
order by deptno;

--Quiz
create table t_emp2(
  id number(2),
  jumin char(7)
);
insert into t_emp2(id, jumin) values(1, '1234567');
insert into t_emp2(id, jumin) values(2, '2234567');
insert into t_emp2(id, jumin) values(3, '3234567');
insert into t_emp2(id, jumin) values(4, '4234567');
insert into t_emp2(id, jumin) values(5, '9234567');
commit;

/*
t_emp2 테이블에서 id, jumin 데이터를 출력하되 jumin 컬럼의 앞자리가 1이면 남성
2이면 여성, 3이면 중성 그 외는 기타로 출력하세요
출력되는 컬럼명은 gender로 하세요
*/

select id, jumin, decode(substr(jumin, 1, 1), 1, '남성',
                                              2, '여성',
                                              3, '중성',
                                              '기타') as "gender"
from t_emp2;

--난이도 중급
--java if() { if() {} } 중첩 if문
--oracle: decode(decode())
--emp table 사용
/*
부서번호가 20번인 사원중에서 SMITH라는 이름을 가진 사원이라면 HELLO 문자 출력하고
부서번호가 20번인 사원중에서 SMITH라는 이름을 가진 사원이 아니라면 WORLD 문자 출력하고
부서번호가 20번인 사원이 아니라면 ETC라는 문자를 출력하세요
*/
select empno, ename, deptno, decode(deptno, '20',
                                            decode(ename, 'SMITH', 'HELLO',
                                                                   'WORLD'),
                                            'ETC') as "난이도 중급"
from emp
order by deptno;

--CASE문
--Java: switch...
/*
CASE 조건 WHEN 결과1 THEN 출력1
                 WHEN 결과2 THEN 출력2
                 WHEN 결과3 THEN 출력3
                 WHEN 결과4 THEN 출력4
                 ELSE 출력5
END "컬럼명"                 
*/
create table t_zip(
  zipcode number(10)
);

insert into t_zip(zipcode) values(2);
insert into t_zip(zipcode) values(31);
insert into t_zip(zipcode) values(32);
insert into t_zip(zipcode) values(33);
insert into t_zip(zipcode) values(41);
commit;

select * from t_zip;

select '0' || to_char(zipcode),
      case zipcode when 2 then '서울'
                   when 31 then '경기'
                   when 32 then '강원'
                   when 41 then '제주'
                   else '기타지역'
      end "regionname"
from t_zip;

/*
사원테이블에서 사원급여가 1000달러 이하면 4급
1001 달러 2000달러 이하면 3급
2001 달러 3000달러 이하면 2급
3001 달러 4000달러 이하면 1급
4001 달러 이상이면 특급을 부여하는 데이터를 출력하세요

1. case 컬럼명 when 결과 then 출력 ( = )
2. case when 컬럼명 조건비교구문 then    (ex: sal < 2000 )
*/

select case when sal <= 1000 then '4급'
            when sal between 1001 and 2000 then '3급'
            when sal between 2001 and 3000 then '2급'
            when sal between 3001 and 4000 then '1급'
            else '특급'
       end "급여등급", empno, ename, sal
from emp;
-----------------------------------------------------------
--문자 >> 숫자 >>날짜 >> 변환(to_char(), to_number(), to_date()) >> 일반함수(nvl(), ~ case)
-->> 집계함수

--오라클.pdf (75Page)

--[집계함수]
--1. count(*), count(컬럼명) >> 데이터 건수
--2. sum()
--3. avg()
--4. max()
--5. min()
--등등

/*
1. 집계함수는 반드시 group by 절과 같이 사용
2. POINT: 모든 집계함수는 null값을 무시
3. POINT: select절에 집계함수 외에 다른 컬럼이 오면 반드시 group by 절에 명시되어야 한다
*/

select count(*) from emp;

select count(empno) from emp; --empno 데이터 몇 건 있지 (단, null 무시)

select count(comm) from emp; --null 무시 >> 6건

select comm from emp;

--
select count(nvl(comm, 0)) from emp; -- nvl함수를 사용하면 >> null 처리

--급여의 합
select sum(sal) from emp; --29025
--평균 급여(급여의 합 / 건수)
select round(avg(sal), 0) from emp; --2073 평균급여

--사장님이 회사 수당이 총 얼마 지급되지(평균)
select sum(comm) as "sum" from emp; -- 4330
select trunc(avg(comm), 0) as "avg" from emp; --721

--회사의 규정이 ... (전체 사원수로 나눈다 ... 309)
--회사의 규정이 ... (받는 사원수로 나눈다 ... 721)

select (300+200+30+300+3500+0) / 6 from dual;
--건수 (null) 무시 >> 6

--검증
select comm from emp;
select (300+200+30+300+3500+0) / 14 from dual; --309
select sum(comm) / 14 from emp;
select sum(comm) / count(*) from emp;
--권장
select avg(nvl(comm, 0)) from emp; --309

select count(*) from emp where comm is null;
-------------------------------------------------------------------
select max(sal) from emp;

select min(sal) from emp;

--POINT: 집계함수의 결과는 [컬럼 1개]
select sum(sal), avg(sal), max(sal), min(sal), count(sal), count(*) from emp;
-------------------------------------------------------------------------------
--부서별 평균 급여를 출력하세요
select deptno, avg(sal)
from emp
group by deptno;

/*
실행순서
select     3
from       1
group by   2
*/
-----------------------------------------------
select depno, avg(sal) from emp; --(x)

select avg(sal)
from emp
group by deptno; --데이터 구분 모호 (알 수 없어요)
-----------------------------------------------
--직종별 평균급여, 급여합, 최대급여, 최소급여, 건수를 출력
select job, avg(sal), sum(sal), max(sal), min(sal), count(sal)
from emp
group by job;

/*
grouping 원리
--distinct 컬럼명1, 컬럼명2
--order by 컬럼명1, 컬럼명2
--group by
*/

--부서별, 직종별 급여의 합을 구하세요
select deptno, job, sum(sal), count(sal)
from emp
group by deptno, job
order by deptno;

/* 실행순서
select      5
from        1
where       2
group by    3
having      4
order by    6
*/

--직종별 평균급여가 3000달러 이상인 사원의 직종과 평균급여를 출력하세요
--평균급여 >= 3000 구해지려면 >> 평균급여 >> 시점 >> group by ...
--where 평균급여 데이터 사용 할 수가 없어요
--답> group by 조건
--group by 조건: having 절

select job, avg(sal)
from emp
group by job
having avg(sal) >= 3000;
--from의 조건절 -> where
--group by의 조건절 -> having

/*
1.사원테이블에서 직종별 급여의 합을 출력하되 수당은 지급받고 급여의 합이 5000 이상인 사원들의 목록을 출력
--급여의 합이 낮은 순으로 출력하세요
*/
select job, sum(sal) as "sumsal"
from emp
where comm is not null
group by job
having sum(sal) >= 5000
order by "sumsal" asc;


/*
2.사원테이블에서 부서 인원이 4명보다 많은 부서의 부서번호, 인원수, 급여의 합을 출력
*/
select deptno, count(*) as "부서별인원", sum(sal) as "부서별 급여의 합"
from emp
group by deptno
having count(*) >= 4; --실행순서상 alias명을 쓸 수 없다.

/*
3.사원테이블에서 직종별 급여의 합이 5000을 초과하는 직종과 급여의 합을 출력하세요
단, 판매직종(salesman)은 제외하고 급여합으로 내림차순 정렬하세요
*/
select job, sum(sal) as sumsal
from emp
where job != 'SALESMAN'
group by job
having sum(sal) > 5000
order by sumsal desc;
-------------------------------------------------------------
--[조인(JOIN)] : 다중테이블에서 데이터를 검색하는 방법
--Oracle.pdf(p85)
--종류
--1. 등가조인(equi join) => 70%
--원테이블과 대응대는 테이블에 있는 컬럼의 데이터를 1:1 매핑
--[ANSI  문법]
--문법 : join  on ~조건절
--문법 : [inner] join on ~조인의 조건절


--2. 비등가조인(non-equi join) => 의미만 존재 => 문법 등가조인
--원테이블과 대응대는 테이블에 있는 컬럼이 1:1 매핑되지 않는 경우
--ex) emp , salgrade : 사원의 등급 검색 컬럼 2개 사용 (losal , hisal)  


--3.outer join(equi join + null) => equi join null 처리 안되요
--outer join (두개의 테이블에서 주 , 종 관계 파악)
--문법 : left outer join  (왼쪽 주 , 오른쪽 종)
--      right outer join  (오른쪽 주 , 왼쪽 종)
--      full outer join   (left , right join > union 하면)


--4. self join (자기참조) => 의미만 존재 => 문법 등가조인
--ex) emp 테이블에서 smith 관리자 이름은 무었입니까
--하나의 테이블안에서 컬럼이 다른 컬럼을 참조하는 경우
---------------------------------------------------------------------
--조인 실습 테이블 구성

create table M (M1 char(6) , M2 char(10));
create table S (S1 char(6) , S2 char(10));
create table X (X1 char(6) , X2 char(10));

insert into M values('A','1');
insert into M values('B','1');
insert into M values('C','3');
insert into M values(null,'3');
commit;

insert into S values('A','X');
insert into S values('B','Y');
insert into S values(null,'Z');
commit;

insert into X values('A','DATA');
commit;

select * from m;
select * from s;
select * from x;
-----------------------------------
select *
from m, s
where m.m1 = s.s1;

--oracle join 문법 (ANSI 문법 x)
select m.m1, m.m2, s.s2
from m, s
where m.m1 = s.s1;

--어떤게 join 조건절이고 어떤것이 from 조건절인지 모호하다
select m.m1, m.m2, s.s2
from m, s
where m.m1 = s.s1 and m.m1 = 'A';

--ANSI 문법 권장
select m.m1, m.m2, s.s2
from m join s
on m.m1 = s.s1;

--조인 요령 (select * ...) >> 필요한 컬럼만 추출

select emp.empno,
       emp.ename,
       emp.deptno,
       dept.dname
from emp join dept
on emp.deptno = dept.deptno;

--테이블에 가명칭 부여 (실제 테이블이름이 길기 때문에, 별칭 사용)
select e.empno,
       e.ename,
       e.deptno,
       d.dname
from emp e join dept d
on e.deptno = d.deptno;

select s.s1, x.x2
from s join x
on s.s1 = x.x1;
---------------------------------------------------------------
--join 한개 이상의 테이블
--oracle sql join 문법
select m.m1, m.m2, s.s2, x.x2
from m, s, x
where m.m1 = s.s1 and s.s1 = x.x1;
--A = B, B = C >> A = C

--ANSI 문법
select m.m1, m.m2, s.s2, x.x2
from m join s on m.m1 = s.s1
       join x on s.s1 = x.x1;

--사번, 이름, 부서번호, 부서명, 급여, 급여등급


----------------------------------------------------------------
--1. HR 계정으로 이동
select * from employees;
select * from departments;
select * from locations;

--employees, departments
--1. 사번, 이름(last_name), 부서번호, 부서이름을 출력하세요
select emp.employee_id, emp.last_name, emp.department_id, dept.department_name
from employees emp join departments dept
on emp.department_id = dept.department_id
order by emp.employee_id;

--문제점: 사원수 107명 >> 출력 106명
--등가조인으로는 해결이 안되요...outer join으로 해결

--2. 사번, 이름, 부서번호, 부서명, 지역코드, 도시명을 출력하세요
select e.EMPLOYEE_ID,
       e.LAST_NAME,
       e.DEPARTMENT_ID,
       d.DEPARTMENT_NAME,
       d.LOCATION_ID,
       l.CITY
from employees e join departments d on e.DEPARTMENT_ID = d.DEPARTMENT_ID
                 join locations l on d.LOCATION_ID = l.LOCATION_ID;

select * from employees where department_id is null;
--------------------------------------------------------------------------------
--비등가 (1:1 매핑 되는 컬럼이 없어요)
select * from emp;
select * from salgrade;

--사번, 이름, 급여, 급여등급을 출력하세요
select e.empno, e.ename, s.grade, e.sal
from emp e join salgrade s
on e.sal between s.losal and s.hisal;

--사번, 이름, 부서번호, 부서명, 급여, 급여등급
--join하는 테이블 3개
select e.empno, e.ename, e.deptno, d.dname, e.sal, s.grade
from emp e join dept d on e.deptno = d.deptno
           join salgrade s on e.sal between s.losal and s.hisal;
        -- join aaa on ...
        -- join bbb on ...
--------------------------------------------------------------------------------
/*
outer join(equi join + null(남는 데이터)) => equi join null 처리 안되요
outer join (두개의 테이블에서 주, 종 관계 파악)

문법:
left outer join (왼쪽 주, 오른쪽 종)
right outer join (오른쪽 주, 왼쪽 종)
full outer join (left, right join > union 하면)

내부적으로 등가조인을 실행하고 (주, 종) 관계를 파악해서
주인이 되는 테이블에 있는 남는 데이터를 가지고 오는 것
*/

select *
from m join s
on m.m1 = s.s1;

select *
from m left outer join s
on m.m1 = s.s1;

select *
from m right outer join s
on m.m1 = s.s1;

select *
from m full outer join s
on m.m1 = s.s1;

--1. HR 계정으로 이동
select * from employees;
select * from departments;
select * from locations;

--employees, departments
--1. 사번, 이름(last_name), 부서번호, 부서이름을 출력하세요
select emp.employee_id, emp.last_name, emp.department_id, dept.department_name
from employees emp join departments dept
on emp.department_id = dept.department_id
order by emp.employee_id;

--문제점: 사원수 107명 >> 출력 106명
--등가조인으로는 해결이 안되요...outer join으로 해결

select * from employees where department_id is null;

--107명이 나오도록 하세요 (남는 데이터)
select emp.employee_id, emp.last_name, emp.department_id, dept.department_name
from employees emp left outer join departments dept
on emp.department_id = dept.department_id
order by emp.employee_id;
--------------------------------------------------------------------------------
--self join (자기참조) -> 문법 >> 등가조인
--하나의 테이블에서 컬럼이 다른 컬럼을 참조하는 경우
select * from emp;

select e.empno, e.ename, m.empno, m.ename
from emp e join emp m
on e.mgr = m.empno;

--사원 14 >> 결과 13명 >> 사장님 mgr데이터값이 null
select * from emp where mgr is null;
--14명의 사원이 나오도록 수정 (outer join)
select e.empno, e.ename, m.empno, m.ename
from emp e left outer join emp m --self join 하나의 테이블을 2개 처리 (테이블에 가명칭 부여)
on e.mgr = m.empno;
--------------------------------------------------------------------------------
select *
from m, s;
--where m.m1 = s.s1
--------------------------------------------------------------------------------
-----[20분]-----------------------[JOIN]----------------------------------------
-- 1. 사원들의 이름, 부서번호, 부서이름을 출력하라.
select e.ENAME, e.DEPTNO, d.DNAME
from emp e join dept d
on e.DEPTNO = d.DEPTNO;

-- 2. DALLAS에서 근무하는 사원의 이름, 직위, 부서번호, 부서이름을
-- 출력하라.
select e.ENAME, e.JOB, e.DEPTNO, d.LOC, d.DNAME
from dept d join emp e
on d.DEPTNO = e.DEPTNO
where d.loc = 'DALLAS';

-- (틀림)3. 이름에 'A'가 들어가는 사원들의 이름과 부서이름을 출력하라.
select e.ename, d.dname
from emp e join dept d
on d.deptno = e.deptno

--정답
select e.ename, d.dname
from emp e join dept d
on d.deptno = e.deptno
where e.ename like '%A%';



-- 4. 사원이름과 그 사원이 속한 부서의 부서명, 그리고 월급을
--출력하는데 월급이 3000이상인 사원을 출력하라.
select e.ename, d.dname, e.sal
from emp e join dept d
on e.deptno = d.deptno
where e.sal >= 3000;


-- 5. 직위(직종)가 'SALESMAN'인 사원들의 직위와 그 사원이름, 그리고
-- 그 사원이 속한 부서 이름을 출력하라.
select e.job, e.ename, d.dname
from emp e join dept d
on e.deptno = d.deptno
where e.job = 'SALESMAN';


-- 6. 커미션이 책정된 사원들의 사원번호, 이름, 연봉, 연봉+커미션,
-- 급여등급을 출력하되, 각각의 컬럼명을 '사원번호', '사원이름',
-- '연봉','실급여', '급여등급'으로 하여 출력하라.
--(비등가 ) 1 : 1 매핑 대는 컬럼이 없다
select e.empno as "사원번호", e.ename as "사원이름", e.sal * 12 as "연봉", (e.sal * 12) + e.comm as "실급여", s.grade as "급여등급"
from emp e join salgrade s
on e.sal between s.losal and s.hisal
where e.comm is not null;

--다른 정답
select e.empno as "사원번호", e.ename as "사원이름", e.sal * 12 as "연봉", (e.sal * 12) + nvl(e.comm, 0) as "실급여", s.grade as "급여등급"
from emp e join salgrade s
on e.sal between s.losal and s.hisal
where e.comm is not null;


-- 7. 부서번호가 10번인 사원들의 부서번호, 부서이름, 사원이름,
-- 월급, 급여등급을 출력하라.
select e.deptno, d.dname, e.ename, e.sal, s.grade
from emp e join dept d on e.deptno = d.deptno
           join salgrade s on e.sal between s.losal and s.hisal
where e.deptno = 10;


-- (틀림)8. 부서번호가 10번, 20번인 사원들의 부서번호, 부서이름,
-- 사원이름, 월급, 급여등급을 출력하라. 그리고 그 출력된
-- 결과물을 부서번호가 낮은 순으로, 월급이 높은 순으로
-- 정렬하라.
select e.deptno, e.ename, e.sal
from emp e
--from emp e join dept d on e.deptno = d.deptno
--           join salgrade s on e.sal between s.losal and s.hisal
where e.deptno = 10 and e.deptno = 20
order by e.deptno asc, e.sal desc;

--정답
SELECT E.DEPTNO, D.DNAME, E.ENAME, E.SAL, S.GRADE
FROM EMP E  join DEPT D on E.DEPTNO=D.DEPTNO
            join SALGRADE S on E.SAL BETWEEN S.LOSAL AND S.HISAL
WHERE  E.DEPTNO<=20 --WHERE E.DEPTNO IN (10,20)  -- e.deptno = 10 or
ORDER BY E.DEPTNO ASC,  E.SAL DESC;


-- (틀림)9. 사원번호와 사원이름, 그리고 그 사원을 관리하는 관리자의
-- 사원번호와 사원이름을 출력하되 각각의 컬럼명을 '사원번호',
-- '사원이름', '관리자번호', '관리자이름'으로 하여 출력하라.
--SELF JOIN (자기 자신테이블의 컬럼을 참조 하는 경우)
select e.empno as "사원번호", e.ename as "사원이름", t.empno as "관리자번호", t.ename as "관리자이름"
from emp e join emp t
on e.mgr = t.empno;

--정답
select e.empno as "사원번호", e.ename as "사원이름", t.empno as "관리자번호", t.ename as "관리자이름"
from emp e left outer join emp t
on e.mgr = t.empno;
--------------------------------------------------------------------------------
--[subquery]오라클.pdf (100page)
--sql 꽃 이라 부른다 >> sql의 만능 해결사

--사원테이블에서 사원들의 평균 월급보다 더 많은 급여를 받는 사원의 사번, 이름, 급여를 출력
--서브
select avg(sal) from emp; -- 2073

--메인
select *
from emp
where sal > (select avg(sal) from emp);
--------------------------------------------------------------------------------
--쿼리 작성 기준
--함수 >>> 조인 >>> subquery (마지막 무기)
/*
subquery
종류
1. single row subquery: subquery의 결과가 1개의 row(단일값): 한개의 값 (단일 컬럼)

2. multi row subquery: subquery의 결과가 1개 이상의 row: 여러개의 값 (단일 컬럼)

구분하는 이유 (사용되는 연산자가 차이)
--multi row
--연산자(in, not in) (any, all) >> 다중 데이터
--all: sal > 1000 and sal > 2000 and ..
--any: sal > 1000 or sal > 2000 or ...

문법(정의)
1. subquery는 괄호 안에 있어야 한다 > (select sal from emp)
2. subquery는 단일 컬럼으로 구성 > (select sal, deptno from emp) >> (x)
3. subquery는 단독으로 실행 가능해야 한다.

subquery를 가지고 있는 sql 문장
1. subquery 먼저 실행
2. subquery의 결과값을 가지고 main 쿼리가 실행된다
*/

--사원테이블에서 jones의 급여보다 더 많은 급여를 받는 사원의 사번, 이름, 급여를 출력하세요
select sal from emp where ename='JONES'; -- 2975

select empno, ename, sal
from emp
where sal > (select sal from emp where ename='JONES');

select *
from emp
--where sal = (select sal from emp where sal > 2000); --안됨
where sal in (select sal from emp where sal > 2000);
--where sal = 2975 or sal = 2850 or sal = 2450 or sal = 3000 or sal = 5000;

select *
from emp
where sal not in (select sal from emp where sal > 2000);
--where sal != 2975 and sal != 2850 and sal != 2450 and....

--부하직원이 있는 사원의 사번과 이름을 출력하세요
select e.empno, e.ename from emp e join emp f on e.empno = f.mgr;

select mgr from emp;

select *
from emp
where empno in (select mgr from emp);
--empno = 7839 or empno = 7566 or empno is null or ...
--or 연산에서 null(x)

--부하직원이 없는 사원의 사번과 이름을 출력하세요
select *
from emp
where empno not in (select mgr from emp); -- 오답
--empno != 7839 and empno != 7566 and empno is not null and ...>> 최종결과 >> null
--null만 나오면 nvl

select *
from emp
where empno not in (select nvl(mgr, 0) from emp);

--KING에게 보고하는 즉 직속상관이 KING인 사원의 사번, 이름, 직종, 관리자 사번을 출력하세요
select empno from emp where ename = 'KING'; --7839

select empno, ename, job, mgr
from emp where mgr = (select empno from emp where ename = 'KING'); -- (=자리에 in 사용해도 됨)

--20번 부서의 사원 중에서 가장 많은 급여를 받는 사원보다 더 많은 급여를 받는 사원의
--사번, 이름, 급여, 부서번호를 출력하세요
select max(sal) from emp where deptno = 20; --3000

select *
from emp
where sal > (select max(sal) from emp where deptno = 20);
--집계함수가 subquery 활용 빈번

select *
from emp
where deptno in (select deptno from emp where job='SALESMAN')
and sal in (select sal from emp where job='SALESMAN');

--QUIZ) 경품: 15분 >> hint 5분 >> hint 5분 (3분)
--중급
/*
1단계 hint) 부서번호와 부서의 평균급여가 있는 테이블이 존재한다면

2단계 hint) from 절에 subquery 사용가능하다 (in line view)  >> 가상테이블 ....

자기 부서의 평균 월급보다 더 많은 월급을 받는 사원의 사번, 부서번호, 부서별 평균 월급을 출력
*/

--자기 부서의 평균 월급을 구하시오.
select e.empno, e.ename, e.deptno, e.sal, trunc(s.avgsal, 0)
from emp e join (select deptno, avg(sal) as avgsal from emp group by deptno) s
on e.deptno = s.deptno
where e.sal > s.avgsal;

--1. 'SMITH'보다 월급을 많이 받는 사원들의 이름과 월급을 출력하라.
select sal from emp where ename = 'SMITH';

select ename, sal
from emp
where sal > (select sal from emp where ename = 'SMITH');

--2. 10번 부서의 사원들과 같은 월급을 받는 사원들의 이름, 월급,
-- 부서번호를 출력하라.
select sal from emp where deptno = 10;

select ename, sal, deptno
from emp
where sal in (select sal from emp where deptno = 10);

--[틀림]3. 'BLAKE'와 같은 부서에 있는 사원들의 이름과 고용일을 뽑는데
-- 'BLAKE'는 빼고 출력하라.
select deptno from emp where ename='BLAKE';

select ename, hiredate
from emp
where deptno in (select deptno from emp where ename='BLAKE');

--정답
select deptno from emp where ename='BLAKE';

select ename, hiredate
from emp
where deptno in (select deptno from emp where ename='BLAKE')
and ename != 'BLAKE';

--4. 평균급여보다 많은 급여를 받는 사원들의 사원번호, 이름, 월급을
-- 출력하되, 월급이 높은 사람 순으로 출력하라.
select avg(sal) from emp;

select empno, ename, sal
from emp
where sal > (select avg(sal) from emp)
order by sal desc;

--5. 이름에 'T'를 포함하고 있는 사원들과 같은 부서에서 근무하고
-- 있는 사원의 사원번호와 이름을 출력하라.
select deptno from emp where ename like '%T%';

select empno, ename
from emp
where deptno in (select deptno from emp where ename like '%T%');

--[??]6. 30번 부서에 있는 사원들 중에서 가장 많은 월급을 받는 사원보다
-- 많은 월급을 받는 사원들의 이름, 부서번호, 월급을 출력하라.
--(단, ALL(and) 또는 ANY(or) 연산자를 사용할 것)

--정답
select sal from emp where deptno = 30;

select ename, deptno, sal
from emp
where sal > all(select sal from emp where deptno = 30);


--7. 'DALLAS'에서 근무하고 있는 사원과 같은 부서에서 일하는 사원의
-- 이름, 부서번호, 직업을 출력하라.
select e.deptno from emp e join dept d on e.deptno = d.deptno where d.loc = 'DALLAS';

select ename, deptno, job
from emp
where deptno in (select e.deptno from emp e join dept d on e.deptno = d.deptno where d.loc = 'DALLAS');


--8. SALES 부서에서 일하는 사원들의 부서번호, 이름, 직업을 출력하라.
select e.deptno from emp e join dept d on e.deptno = d.deptno where d.dname = 'SALES';

select deptno, ename, job
from emp
where deptno in (select e.deptno from emp e join dept d on e.deptno = d.deptno where d.dname = 'SALES');

--9. 'KING'에게 보고하는 모든 사원의 이름과 급여를 출력하라
--king 이 사수인 사람 (mgr 데이터가 king 사번)
select empno from emp where ename = 'KING';

select ename, sal
from emp
where mgr in (select empno from emp where ename = 'KING');


--10. 자신의 급여가 평균 급여보다 많고, 이름에 'S'가 들어가는
-- 사원과 동일한 부서에서 근무하는 모든 사원의 사원번호, 이름,
-- 급여를 출력하라.
select avg(sal) from emp;
select deptno from emp where ename like '%S%';

select empno, ename, sal
from emp
where sal > (select avg(sal) from emp)
and deptno in (select deptno from emp where ename like '%S%');


--11. 커미션을 받는 사원과 부서번호, 월급이 같은 사원의
-- 이름, 월급, 부서번호를 출력하라.
select sal from emp where comm is not null and comm != 0;

select ename, sal, deptno
from emp
where sal in (select sal from emp where comm is not null and comm != 0);


--12. 30번 부서 사원들과 월급과 커미션이 같지 않은
-- 사원들의 이름, 월급, 커미션을 출력하라.
select sal from emp where deptno = 30;
select comm from emp where deptno = 30;

select ename, sal, comm
from emp
where sal not in (select sal from emp where deptno = 30)
and comm not in (select nvl(comm, 0) from emp where deptno = 30);
--------------------------------------------------------------------------------
--[INSERT, UPDATE, DELETE]
/*
오라클 기준
DDL(데이터 정의어): create, alter, drop, truncate (rename, modify)
DML(데이터 조작어): insert, update, delete
DQL(데이터 질의어): select
DCL(데이터 제어어): 권한(grant, revoke)
TCL(트랜잭션): commit, rollback, savepoint

*/
--DML 작업
--트랜잭션(transaction): 하나의 논리적인 작업 단위
--트랜잭션으로 처리되어야 하는 업무
--은행업무라면...(A라는 계좌에서 돈을 출금, B라는 계좌에 이체)
--[A 출금 ~~~~~~~~~~~ B 이체] 하나의 업무: 결과 > [성공, 실패]
--commit (A ~ B 예외없이 실행되면), rollback (A ~ B 진행중 예외 발생하면)

/*
  A라는 계좌 (100만원 출금): UPDATE ...
  ...
  ...
  B라는 계좌 (100만원 입금): UPDATE ...
*/

--테이블 기본 정보
desc emp;
--오라클 (system table(dictionary) 통해서 다양한 정보 제공)
select * from tab; --현재 접속한 계정 (BIT_USER) 볼 수 있는 테이블 목록

select * from tab where tname = 'BOARD'; --테이블생성 전에 그 정보를 확인

select * from col; --BIT_USER 사용자가 관리하는 모든 컬럼 정보
select * from col where tname='EMP'; --특정 테이블이 가지는 컬럼 정보

select * from user_tables; --관리자, 튜닝 정보..
select * from user_tables where table_name='DEPT';
--------------------------------------------------------------------------------
--DML(오라클.pdf > 168page)

/*
INSERT INTO table_name [(column1[, column2, . . . . . ])]
VALUES  (value1[, value2, . . . . . . ]);
*/

--무결성:
create table temp(
  id number primary key, --id 컬럼에 null(x), 중복데이터(x), 유일한 데이터 1건 보장: where id = 10
  name varchar2(20) --default null 허용
);

--1. 가장 일반적인...
insert into temp(id, name)
values(100, '홍길동');

select * from temp;

--반영
commit;

--2. 컬럼 리스트 생략(temp(id, name) > temp 조건>> 모든 컬럼에 데이터 삽입시
insert into temp --컬럼리스트 생략 가능하지만 쓰는 것을 권장
values(200, '김유신');

select * from temp;
commit;

--1. error
insert into temp(id, name)
values(100, '아무개'); --중복 데이터 PK문제 발생
--unique constraint (BIT_USER.SYS_C007003) violated --> pk 제약 위반

--2. error
insert into temp(name)
values('아무개');
--cannot insert NULL into ("BIT_USER"."TEMP"."ID") --> pk (null값 안되요)
--------------------------------------------------------------------------------
--insert 데이터 어느정도
--일반 SQL은 제어문 없어요

--재미^^
--pl-sql
create table temp2(id varchar2(20));

/*
BEGIN
  FOR i IN 1..1000 LOOP
    insert into temp2(id) values('A'||to_char(i));
  END LOOP;
END;
*/

select * from temp2;
--------------------------------------------------------------------------------
create table temp3(
  memberid number(3) not null,
  name varchar2(10), --null 허용
  regdate date default sysdate --기본값 설정하기 (insert 하지않으면 기본값으로 서버 날짜 적용)
);

select sysdate from dual;

insert into temp3(memberid, name, regdate)
values(100, '홍길동', '2018-03-02');

select * from temp3;

--특정 컬럼에 데이터 넣기
insert into temp3(memberid, name)
values(200, '아무개'); --default가 동작 (regdate 컬럼 >> sysdate 값이 insert)

select * from temp3;
commit;

insert into temp3(memberid)
values(300);

select * from temp3;
commit;
--------------------------------------------------------------------------------
--옵션 TIP)
--1. 대량 데이터 INSERT 하기
create table temp4(id number);
create table temp5(num number);

insert into temp4(id) values(1);
insert into temp4(id) values(2);
insert into temp4(id) values(3);
insert into temp4(id) values(4);
insert into temp4(id) values(5);
insert into temp4(id) values(6);
insert into temp4(id) values(7);
insert into temp4(id) values(8);
insert into temp4(id) values(9);
insert into temp4(id) values(10);
commit;

select * from temp4;

--요구사항: temp4 테이블에 있는 모든 데이터를 temp5에 넣고 싶어요
--insert into 테이블명(컬럼리스트) select 구문 (values 구문 대신에)
--단, 컬럼개수와 타입이 일치 한다면...

insert into temp5(num)
select id from temp4;

select * from temp5;
commit;

--2. INSERT
--테이블이 없는 상황에서 [테이블자동생성][대량 데이터 삽입]
--단, 제약 정보는 복사가 안되요 (PK, FK ...)

--emp테이블과 같은 구조를 가지고 같은 데이터를 갖는 실습 테이블
create table copyemp
as
  select * from emp;

select * from copyemp;
--
create table copyemp2 --empno, ename, job, sal 컬럼 생성
as
  select empno, ename, job, sal from emp where deptno=30;

select * from copyemp2;

--질문) 구조(틀)만 복사하고 데이터는 복사하고 싶지 않아요
create table copyemp3
as
  select * from emp where 1 = 2; --where 조건을 거짓으로 만든다

select * from copyemp3;
--주의 사항(단점)
--시스템 테이블 사용(제약 정보 확인하기)
select * from user_constraints;

select * from user_constraints where table_name = 'EMP';
select * from user_constraints where table_name = 'COPYEMP';

create table pktest(id number primary key);
insert into pktest(id) values(100);
commit;

create table pktest2
as
  select * from pktest;

select * from pktest2;
desc pktest;
desc pktest2;
select * from user_constraints where table_name = 'PKTEST';
select * from user_constraints where table_name = 'PKTEST2';
-----------------------[INSERT END]---------------------------------------------
--[UPDATE]
/*
UPDATE table_name
SET  column1 = value1 [,column2 = value2, . . . . . . .]
[WHERE  condition];

서브쿼리 사용
UPDATE  table_name
SET  (column1, column2, . . . . ) = ( SELECT  column1,column2, . . .
FROM   table_name
WHERE  coundition) [WHERE  condition];
*/
commit;
select * from copyemp;

update copyemp
set sal=0;

select * from copyemp;

rollback;

select * from copyemp;

update copyemp
set job='NOT.....'
where deptno=20;

select * from copyemp order by deptno;

rollback;

select * from copyemp order by deptno;

--update(subquery 사용: 데이터 변경)
update copyemp
set sal = (select sum(sal) from emp);

select * from copyemp;
rollback;

update copyemp
set ename = 'AAAA', job = 'BBBB', hiredate = sysdate
where deptno = 10;

select * from copyemp where deptno = 10;
------------------------------[UPDATE END]--------------------------------------
--[DELETE]
delete from copyemp;

select * from copyemp;

rollback;

delete from copyemp where deptno=20;

select * from copyemp where deptno=20;

rollback;
------------------------------[DELETE END]--------------------------------------
/*
APP(JAVA) --> JDBC API --> ORACLE
CRUD 작업
--Create: insert
--Read  : select (전체조회, 조건조회 (row 1건))
--Update: update
--Delete: delete
(Create, Update, Delete >> 트랜잭션을 동반(comiit, rollback))

--전체조회, 조건조회, 삭제, 수정, 삽입 (개발자)
--함수 최소 5개
예)JAVA
public List<Emp> getEmpList() { 안에 쿼리문 select * from emp }
public Emp getEmpListByEmpno(int empno) { select ...}
public int insertEmp(Emp emp) { intsert into emp(...) }

위 작업: JDBC
*/
--------------------------------------------------------------------------------
--[DDL] create, alter, drop (기준 테이블)
select * from user_tables where lower(table_name) = 'board';

drop table board;

create table board(
  boardid number,
  title varchar2(50),
  content varchar2(2000),
  regdate date
);

desc board;

--편하게 살기 아래 2개 정도
select * from user_tables where lower(table_name) = 'board';
select * from user_constraints where lower(table_name) = 'board';

--Oracle 11g 가상컬럼(조합 컬럼)
--학생성적: 국어, 영어, 수학, [총점] 컬럼이 있어요
--국어, 영어,수학 데이터만 insert되면 (국어 + 영어 + 수학) 자동화

create table vtable(
  no1 number,
  no2 number,
  no3 number GENERATED ALWAYS AS (no1 + no2) VIRTUAL
);

insert into vtable(no1, no2) values(100, 50);
insert into vtable(no1, no2) values(80, 60);

select * from vtable;
desc vtable;

--no3 컬럼에 데이터를 직접 넣을 수 있을까요?
insert into vtable(no1, no2, no3) values(80, 60, 200); --불가

select COLUMN_NAME, DATA_TYPE, DATA_DEFAULT
from user_tab_columns where table_name='VTABLE';

--실무에 사용되는 형식의 코드
--제품정보 (입고일) 기준으로 분기데이터 (4분기)
--입고일: 2018-03-02 >> 1분기
create table vtable2(
  no number,  --순번
  p_code char(4), --제품코드(A001, B002, ...)
  p_date char(8), --입고일(20180302)
  p_qty number, --수량
  p_bungi number(1) --가상컬럼
          GENERATED ALWAYS AS (
            CASE WHEN substr(p_date, 5, 2) IN ('01', '02', '03') THEN 1
                 WHEN substr(p_date, 5, 2) IN ('04', '05', '06') THEN 2
                 WHEN substr(p_date, 5, 2) IN ('07', '08', '09') THEN 3
                 ELSE 4
            END
          ) VIRTUAL
);

insert into vtable2(p_date) values('20180101');
insert into vtable2(p_date) values('20180126');
insert into vtable2(p_date) values('20180403');
insert into vtable2(p_date) values('20181101');
insert into vtable2(p_date) values('20182101');
commit;

select * from vtable2;
--------------------------------------------------------------------------------
--DDL 테이블 다루기...(제약정보)...
--오라클.pdf(140 Page)

--1. 테이블 생성하기
create table temp6(id number);

--2. 테이블 생성했는데 컬럼 추가하기
--기존 테이블에 컬럼 추가하기
desc temp6;

alter table temp6
add ename varchar2(20);

desc temp6;

--3. 기존 테이블에 있는 컬럼의 이름 잘못표기(ename -> username)
--기존 테이블에 있는 컬럼의 이름 바꾸기(rename)

alter table temp6
rename column ename to username;

desc temp6;

--4. 기존 테이블에 있는 기존 컬럼의 Type 수정
--(modify)
alter table temp6
modify(username varchar2(200));

desc temp6;

--5. 기존 테이블에 있는 기존 컬럼 삭제
alter table temp6
drop column username;

desc temp6;

--6. 테이블 데이터 삭제: delete
--테이블 처음 만들면 처음 크기 >> 데이터 넣으면 >> 데이터 크기
--ex) 처음 1M >> 10만건 >> 100M >> 10만건 >> delete >> 현재 테이블 크기 101M
--테이블 데이터 삭제, [공간의 크기]를 줄일 수 있을까?
--truncate (where 조건 사용 못해요)
--ex) 처음 1M >> 10만건 >> 100M >> 10만건 >> truncate >> 현재 테이블 크기 1M

drop table temp6;

desc temp6;
--------------------------------------------------------------------------------
--eXERD 프로그램 활용 (http://ko.exerd.com)
--------------------------------------------------------------------------------
--[테이블 제약 설정하기]
--오라클.pdf (144page)
--데이터베이스 무결성 확보
--제약 (constraint): insert, update 주로 적용

/*
*NOT NULL(NN) 열은 NULL 값을 포함할 수 없습니다.
*UNIQUE key(UK) 테이블의 모든 행을 [유일하게 하는 값을 가진 열(NULL 을 허용)]
--unique 제약은 null값을 가질 수 있다 >> null값을 못가지게 하려면 (not null 포함)

*PRIMARY KEY(PK) 유일하게 테이블의 각행을 식별(NOT NULL 과 UNIQUE 조건을 만족)
--not null하고 unique한 제약 (내부적으로 index가 자동 설정)
--index는 검색에 초점, 유지보수에는 불리

*FOREIGN KEY(FK) 열과 참조된 열 사이의 외래키 관계를 적용하고 설정합니다.
(참조제약) [테이블]과 [테이블]간의 관계 설정

*CHECK(CK) 참이어야 하는 조건을 지정함(대부분 업무 규칙을 설정)  
설정한 범위 내의 값만 입력받겠다 (gender: 컬럼에 '남' 또는 '여'라는 데이터 만)
ex) where gender in ('남', '여')

제약을 만드는 시점
1. 테이블 만들면서 바로 생성 (create table ...)
2. 테이블 생성 이후 (alter table ... add constraint...) >> 자동 생성 툴 (exerd)
*/

select * from user_constraints where table_name='EMP';
--------------------------------------------------------------------------------
create table temp7
(
  --줄임 표현
  --id number primary key, --권장하지 않음 --제약에 이름이 자동 등록 >> SYS_C006997
  id number constraint pk_temp7_id PRIMARY KEY, --관용적 표현: pk_테이블명_컬럼명
  name varchar2(20) not null,
  addr varchar2(50)
);

select * from user_constraints where table_name='TEMP7';

insert into temp7(name, addr) values('홍길동', '서울시 강남구'); --(x)

insert into temp7(id, name, addr) values(10, '홍길동', '서울시 강남구'); --(o)

insert into temp7(id, name, addr) values(10, '김유신', '서울시 강남구'); --중복키값 error
commit;
--------------------------------------------------------------------------------
create table temp8
(
  id number constraint pk_temp8_id primary key, --PK는 테이블 당 한개(중복키 가능: 묶어서 한개)
  name varchar2(20) not null,
  jumin char(6) constraint uk_temp8_jumin unique, --null 허용
  --jumin char(6) not null constraint uk_temp8)jumin unique (not null하고 unique한 제약)
  addr varchar2(20)
);

select * from user_constraints where table_name = 'TEMP8';

insert into temp8(id, name, jumin, addr)
values(10, '홍길동', '123456', '경기도');

commit;

select * from temp8;

insert into temp8(id, name, jumin, addr)
values(100, '김유신', '123456', '경기도'); --중복키 error
--unique constraint (BIT_USER.UK_TEMP8_JUMIN) violated

insert into temp8(id, name, jumin)
values(10, '아무개', '123456');

--null 값 허용할까요^^
select * from temp8;
insert into temp8(id,name,addr) values(200,'235467', '서울시 강남구');
--------------------------------------------------------------------------------
--테이블 생성 후에 제약 걸기
create table temp9(id number);

--기존 테이블에 제약 추가하기
--주의) 입력된 데이터가 있다면 >> 10, 10 두 건 >> pk 제약 (x) >> 데이터 삭제 >> 제약

alter table temp9
add constraint pk_temp9_id primary key(id);

select * from user_constraints where table_name='TEMP9';

select * from temp9;

alter table temp9
add ename varchar2(20);

--테이블 기본 정보
desc temp9;
--테이블 제약 정보
select * from user_constraints where table_name='TEMP9';

--not null 제약 추가하기
alter table temp9
modify(ename not null);

desc temp9;
--------------------------------------------------------------------------------
--[check]
--where 조건과 동일한 형태 >> where gender in ('남', '여');
create table temp10(
  id number constraint pk_temp10_id primary key, --pk는 테이블 당 1개 (묶어서 1개)
  name varchar2(20) not null, --필수 입력
  jumin char(6) constraint uk_temp10_jumin unique, --필요하다면 not null 추가 가능
  addr varchar2(20), --추가 입력
  age number constraint ck_temp10_age check(age >= 19) -- where age >= 19;
);

--테이블 기본 정보
desc temp10;
--테이블 제약 정보
select * from user_constraints where table_name='TEMP10';

insert into temp10(id, name, jumin, addr, age)
values(100, '홍길동', '123456', '서울시 강남구', 25);

select * from temp10;

insert into temp10(id, name, jumin, addr, age)
values(200, '홍길동', '123457', '서울시 강남구', 25);

insert into temp10(id, name, jumin, addr, age)
values(300, '홍길동', '123458', '서울시 강남구', 18); --check(x)

commit;

select * from temp10;
--------------------------------------------------------------------------------
--[참조제약] >> 테이블과 테이블과의 제약 >> 관계 (RDB)

create table c_emp
as
  select empno, ename, deptno from emp where 1=2;

create table c_dept
as
  select deptno, dname from dept where 1=2;

select * from c_emp;
select * from c_dept;

--참조키 (c_emp (deptno) fk -> c_dept(deptno) 컬럼을 참조 하도록...)
--참조를 당하려면 (데이터를 빌려 주려면) >> 신용 >> 최소한 unique 또는 primary key 확보
--2번 실행)
alter table c_emp
add constraint fk_emp_deptno foreign key(deptno) references c_dept(deptno);

--순서 (c_dept (deptno) pk 걸고
--그리고 c_emp (deptno) 참조
--1번 실행)
alter table c_dept
add constraint pk_dept_deptno primary key(deptno);

insert into c_dept(deptno, dname) values(100, '인사팀');
insert into c_dept(deptno, dname) values(200, '관리팀');
insert into c_dept(deptno, dname) values(300, '회계팀');
commit;

select * from c_dept;
select * from c_emp;

--사원 입사 (부서정하지 않고)
insert into c_emp(empno, ename)
values(100, '홍길동'); --fk not null을 강제하지 않아요 (null)

select * from c_emp;

insert into c_emp(empno, ename, deptno) -- fk 위반 (부서는 100, 200, 300만 있는데 500번 위반)
values(200, '김유신', 500); --(x)

insert into c_emp(empno, ename, deptno)
values(200, '김유신', 300);

select * from c_emp;
select * from c_dept;
--deptno라는 관계에서 (c_dept(부모: master) - c_emp(자식: detail))
commit;

delete from c_dept where deptno = 300; --(x)
delete from c_dept where deptno = 200; --(o)
--300번 삭제 >> 참조하고 있는 쪽 (c_emp 부터 삭제하고) >> c_dept >> 300부서 삭제
--------------------------------------------------------------------------------
--오라클.pdf 145p
/*
column datatype [CONSTRAINT constraint_name]
REFERENCES table_ name (column1[,column2,..] [ON DELETE CASCADE])

column datatype, . . . . . . . , [CONSTRAINT constraint_name] FOREIGN KEY (column1[,column2,..])
REFERENCES table_name  (column1[,column2,..] [ON DELETE CASCADE])
*/
-- [ON DELETE CASCADE] 부모테이블과 생명을 같이 하겠다
-- 300 삭제할 수 있나요? 네
-- delete from c_dept where deptno = 300;
-- 참조하는 자식 데이터 같이 삭제
-- delete from c_emp where deptno = 300; 도 같이 실행됨

alter table c_emp
add constraint fk_emp_deptno foreign key(deptno) references c_dept(deptno) ON DELETE CASCADE;
------------------------------[제약 END]-----------------------------------------
--[SQL 조별과제]
--학생 성적 테이블
--학번의 데이터는 중복되거나 NULL 값을 허용하면 안된다
--이름 NULL 값을 허용하지 않는다
--국어
--영어
--수학 점수 컬럼은 숫자 타입이고 NULL 값을 허용
--는 값을 입력하지 않으면  default로 0값을 갖는다
--총점 ,평균 컬럼은 가상컬럼으로(조합컬럼) 생성한다
--학과코드는 학과 테이블에 학과코드를 참조한다
--학번 , 이름 , 국어 , 영어 , 수학 , 총점 , 평균 , 학과코드

--학과 테이블
--학과코드 데이터는 중복되거나 NULL 값을 허용하면 안된다,
--학과명 은 null값을 허락하지 않는다

--학과코드 , 학과명

--그리고 select 결과는
--학번 , 이름  총점, 평균 , 학과코드 , 학과명 을 출력하세요

create table std_score ( --학생 성적 테이블
  stdno number constraint pk_std_score_stdno primary key,
  name varchar2(10) not null,
  sub_kor number default 0,
  sub_eng number default 0,
  sub_math number default 0,
  sum number GENERATED ALWAYS AS (sub_kor + sub_eng + sub_math) VIRTUAL,
  avg number GENERATED ALWAYS AS ((sub_kor + sub_eng + sub_math) / 3) VIRTUAL,
  deptcode varchar2(20)
);

desc std_score; --테이블 기본 정보
select * from user_constraints where table_name='STD_SCORE'; --테이블 제약 정보

create table std_dept ( --학과 테이블
  deptcode varchar2(20) constraint pk_std_dept_deptcode primary key,
  deptname varchar2(20) not null
);

desc std_dept;
select * from user_constraints where table_name='STD_DEPT';

alter table std_score
add constraint fk_std_score_deptcode foreign key(deptcode) references std_dept(deptcode);

insert into std_score(stdno, name, sub_kor, sub_eng, sub_math, deptcode)
values(100, '홍길동', 90, 30, 40, 'A001');

select * from std_score;

-- 수학성적 생략했을 경우
insert into std_score(stdno, name, sub_kor, sub_eng, deptcode)
values(200, '김유신', 50, 40, 'B001');

insert into std_dept(deptcode, deptname)
values('A001', '해바라기반');

insert into std_dept(deptcode, deptname)
values('B001', '튤립반');

select * from std_dept;
commit;

--결과 출력
select s.stdno as "학번", s.name as "이름", s.sum as "총점", round(s.avg, 2) as "평균", d.deptcode as "학과코드", d.deptname as "학과명"
from std_score s join std_dept d
on s.deptcode = d.deptcode;
--------------------------------------------------------------------------------
--다른 방법 1
create table grade(
  s_number number ,
  s_name varchar2(20) not null,
  k_score number default 0,
  e_score number default 0,
  m_score number default 0,
  m_code number,
  avg_score number GENERATED always as (round((k_score + e_score + m_score)/3,2)) VIRTUAL,
  sum_score number GENERATED always as (k_score + e_score + m_score) VIRTUAL,

  constraint pk_grade_snum primary key(s_number),
  constraint fk_grade_mcode foreign key(m_code) references major(m_code)
);

create table major(
  m_code number,
  m_name varchar2(20) not null,
  constraint pk_major_mcode primary key(m_code)
);

--다른 방법 2
--------------------------------------------------------------------------------
/* 사원 */
CREATE TABLE EMP (
	empno NUMBER NOT NULL, /* 사번 */
	ename VARCHAR2(20), /* 이름 */
	sal NUMBER, /* 급여 */
	deptno NUMBER /* 부서번호 */
);

ALTER TABLE EMP
ADD 		CONSTRAINT PK_EMP 		PRIMARY KEY (empno);

/* 부서 */
CREATE TABLE DEPT (
	deptno NUMBER NOT NULL, /* 부서번호 */
	dname VARCHAR2(20) /* 부서명 */
);

ALTER TABLE DEPT
ADD 	CONSTRAINT PK_DEPT 		PRIMARY KEY (deptno	);

ALTER TABLE EMP
ADD 	CONSTRAINT FK_DEPT_TO_EMP 	FOREIGN KEY (	deptno	)		REFERENCES DEPT (	deptno);
--------------------------------------------------------------------------------
--------------------------------------------------------------------------------
--VIEW(가상테이블) 오라클.pdf (192Page)
--가상테이블
--View 객체(create ...)
--create view 뷰이름 as [view 보는 목록 (select 구문)]
--view는 테이블처럼 사용 가능 (가상 테이블) -> 실제 emp, dept 같은 물리적인 테이블이 아니다
--view는 메모리상에만 존재하는 가상 테이블 (subquery -> in line view -> from (select...))
--view는 sql문자 덩어리

--view 가상 테이블
--사용법: 일반 테이블과 사용법이 동일 from 절 where ...
--DML(insert, update, delete) -> View를 통해서 DML 가능 -> 실제 view를 통해 볼 수 있는 실 테이블 데이터 변경

--view 사용
--개발자의 편리성 (join, subquery) 쉽게 접근 사용 (가상 테이블)
--복잡한 쿼리 단순화
--보안 (권한 별로 처리) -> 노출하고 싶은 데이터만 모아서 view 생성 -> 제공

create view v_001
as
  select empno, ename from emp;

select * from v_001;
select * from v_001 where empno = 7788;

create view v_002
as
  select e.empno, e.ename, e.sal, e.deptno, d.dname
  from emp e join dept d
  on e.deptno = d.deptno;

select * from v_002;

create view v_003
as
  select deptno, avg(sal) as avgsal
  from emp
  group by deptno;

select * from v_003;

select *
from emp e join v_003 s
on e.deptno = s.deptno
where e.sal > s.avgsal;

--만약 원하는 데이터가 테이블로 존재한다면
--JOIN하기 편할 텐데 >> in line view, view생성해서 사용

/*
CREATE  [OR  REPLACE]  [FORCE | NOFORCE]  VIEW view_name [(alias[,alias,...])]
AS Subquery
[WITH  CHECK  OPTION  [CONSTRAINT  constraint ]]
[WITH  READ  ONLY]

OR  REPLACE  이미 존재한다면 다시 생성한다.
FORCE   Base Table 유무에 관계없이 VIEW 을 만든다.
NOFORCE  기본 테이블이 존재할 경우에만 VIEW 를 생성한다.

view_name  VIEW 의 이름
Alias   Subquery 를 통해 선택된 값에 대한 Column 명이 된다.
Subquery  SELECT 문장을 기술한다.

WITH CHECK OPTION VIEW 에 의해 액세스 될 수 있는 행만이 입력,갱신될 수 있다.  
Constraint  CHECK OPTON 제약 조건에 대해 지정된 이름이다.
WITH READ ONLY  이 VIEW 에서 DML 이 수행될 수 없게 한다.
*/

--1. create or replace v_007 as 구문... (수정: overwrite)
--2
/* view 는 컬럼명이 필요하다
drop view v_007;
create or replace view v_007
as
    select avg(sal) as avgsal from emp;
*/

create or replace view v_emp
as
  select empno, ename, deptno from emp where deptno = 20;

select * from v_emp;
select * from v_emp where deptno=10; --볼 수 없다

--view 가상 테이블 >> DML(insert, update, delete ....)
--view [통한] 바라볼 수 있는 데이터에 대해서 DML 가능
--단일 테이블만 (DML) >> view를 통해서...

select * from v_emp;

delete from v_emp;
--실제로는 emp 테이블 >> deptno번호가 20번 삭제

select empno, ename, deptno from emp where deptno = 20;
rollback;
--------------------------------------------------------------------------------
--VIEW 문제
--1. 30번 부서 사원들의 직위, 이름, 월급을 담는 VIEW를 만들어라.
create view view_p1
as
  select job, ename, sal from emp where deptno = 30;

select * from view_p1;

--2. 30번 부서 사원들의  직위, 이름, 월급을 담는 VIEW를 만드는데,
-- 각각의 컬럼명을 직위, 사원이름, 월급으로 ALIAS를 주고 월급이
-- 300보다 많은 사원들만 추출하도록 하라.
-- create or replace view view001 (컬럼명, 컬럼명, .....)  
create or replace view view_p1("부서명", "직위", "사원이름", "월급")
as
  select deptno, job, ename, sal
  from emp
  where deptno = 30 and sal > 300;

select * from view_p1;

--3. 부서별 최대월급, 최소월급, 평균월급을 담는 VIEW를 만들어라.
create or replace view view_p2("부서명", "최대월급", "최소월급", "평균월급")
as
  select deptno, max(sal), min(sal), avg(sal)
  from emp
  group by deptno;

select * from view_p2;

--4. 부서별 평균월급을 담는 VIEW를 만들되, 평균월급이 2000 이상인
-- 부서만 출력하도록 하라.
create or replace view view_p3("부서명", "평균월급")
as
  select deptno, avg(sal)
  from emp
  group by deptno;

select * from view_p3 where "평균월급" >= 2000;

--다른 방법
create or replace view view_p3("부서명", "평균월급")
as
  select deptno, avg(sal)
  from emp
  group by deptno
  having avg(sal) >= 2000;

select * from view_p3;

--5. 직위별 총월급을 담는 VIEW를 만들되, 직위가 MANAGER인
-- 사원들은 제외하고 총월급이 3000이상인 직위만 출력하도록 하라.
create or replace view view_p4("직위명", "총월급")
as
  select job, sum(sal)
  from emp
  where job != 'MANAGER'
  group by job;

select * from view_p4 where "총월급" >= 3000;

--다른 방법
create or replace view view_p4("직위명", "총월급")
as
  select job, sum(sal)
  from emp
  where job != 'MANAGER'
  group by job
  having sum(sal) >= 3000;

select * from view_p4;
--------------------------------------------------------------------------------
/*
오라클.pdf  185Page

시퀀스 객체
순번 추출 (채번기)
자동으로 번호를 생성하는 객체

CREATE  SEQUENCE  sequence_name  
[INCREMENT  BY  n]  
[START  WITH  n]  
[{MAXVALUE n | NOMAXVALUE}]  
[{MINVALUE n | NOMINVALUE}]  
[{CYCLE | NOCYCLE}]  
[{CACHE | NOCACHE}];

sequence_name  SEQUENCE 의 이름입니다.
INCREMENT  BY  n 정수 값인 n 으로 SEQUENCE 번호 사이의 간격을 지정.
이 절이 생략되면 SEQUENCE 는 1 씩 증가.
START  WITH  n  생성하기 위해 첫번째 SEQUENCE 를 지정.
이 절이 생략되면 SEQUENCE 는 1 로 시작.
MAXVALUE n  SEQUENCE 를 생성할 수 있는 최대 값을 지정.
NOMAXVALUE   오름차순용 10^27 최대값과 내림차순용-1 의 최소값을 지정.
MINVALUE n  최소 SEQUENCE 값을 지정.
NOMINVALUE  오름차순용 1 과 내림차순용-(10^26)의 최소값을 지정.
CYCLE | NOCYCLE  최대 또는 최소값에 도달한 후에 계속 값을 생성할 지의 여부를
    지정. NOCYCLE 이 디폴트.
CACHE | NOCACHE  얼마나 많은 값이 메모리에 오라클 서버가 미리 할당하고 유지
    하는가를 지정. 디폴트로 오라클 서버는 20 을 CACHE.  
*/

--게시판
/*
create table board(
   boardid number primary key ,
   title varchar2(50).......
);
boardid >> 중복값 , null  값이 (x)
개발자 : 데이터를 insert  중복값이 아니라는 것에 대한 보장...

where boardid = 10 ; 하나의 row 를 return 하는 것을 보장

게시판 글쓰기
insert into board (...)  values( , '홍길동', '방가방가')

*/
create table kboard (
num number constraint pk_kboard_num primary key,
title varchar2(50)
);
--처음 글을 쓰면   1 이라는 값이 ,  그 다음부터 글을 쓰면 2, 3 , 4 순차적인 값이 .....

--처음글  >> 1
insert into kboard(num,title)
values((select count(num)+1 from kboard),'처음');
--다음글 >> 2 , 3 , 4 ....
insert into kboard(num,title)
values((select count(num) +1 from kboard),'두번째');

insert into kboard(num,title)
values((select count(num) +1 from kboard),'세번째');

select * from kboard;
commit;

--DML 작업 (중복 데이터 위험)......
delete from kboard where  num=1;

select * from kboard;

insert into kboard(num,title)
values((select count(num) +1 from kboard),'네번째');

--delete 삭제 ,,,,,
--insert 중복 (pk) 위반...
---------------------------------------------------
delete from kboard;
commit;

--다른 방법
select * from kboard;

insert into kboard(num,title)
values((select nvl(max(num),0) +1 from kboard),'처음');

insert into kboard(num,title)
values((select nvl(max(num),0) +1 from kboard),'두번');


insert into kboard(num,title)
values((select nvl(max(num),0) +1 from kboard),'세번');

commit;

delete from kboard where num=1;

commit;

select * from kboard;

insert into kboard(num,title)
values((select nvl(max(num),0) +1 from kboard),'네번');

------------------------------------------------------------
--번호를 추출 중복값이 안나오게 순차적인 값
-------------------------------------------------------------
--sequence 객체
create sequence board_num;

select * from user_sequences;
/*
1.4.1 NEXTVAL 과 CURRVAL 의사열
가) 특징
1) NEXTVAL 는 다음 사용 가능한 SEQUENCE 값을 반환 한다.
2) SEQUENCE 가 참조될 때 마다, 다른 사용자에게 조차도 유일한 값을 반환한다.
3) CURRVAL 은 현재 SEQUENCE 값을 얻는다.
4) CURRVAL 이 참조되기 전에 NEXTVAL 이 사용되어야 한다.
*/

--실채번
select board_num.nextval from dual;

--현재값 ... (채번 하지 않고 정보만)
select board_num.currval from dual;


create table tboard(
  num number CONSTRAINT pk_tboard_num PRIMARY KEY,
  title varchar2(50)
);

create sequence tboard_num;

insert into tboard(num, title) values(tboard_num.nextval, '글쓰기');

select tboard_num.currval from dual;

select * from tboard;

delete from tboard where num = 5;
select * from tboard;


insert into tboard(num, title) values(tboard_num.nextval, '글쓰기');
rollback;

--다시 insert하면 새 번호를 (sequence rollback 되는 자원이 아니에요)

--게시판 (10개)
--상품 게시판, 관리자 게시판, 회원 게시판 ...
--테이블 10개 순번은 sequence 객체 하나로 사용해도 된다.
--TIP
--ms-sql: create table board(boardnum int identity(1, 1), title varchar(20))
--        insert into board(title) values('제목')
--2012 버전부터: 오라클 처럼 sequence

--my-sql: create table board(boardnum int auto_increment, title varchar(20))
--        insert into board(title) values('제목')
--------------------------------------------------------------------------------
create sequence seq_num
start with 10
increment by 2;

select seq_num.nextval from dual;
select * from user_sequences; -- LAST_NUMBER는 리셋할 수 없다. 다시 sequence생성해서 사용해야 한다.

--오라클 sequence
--100건 insert -> sequence -> 100번 채번 -> 50건 삭제 -> insert하면 101번 생성
--1,2,3,4,5,6,..., 50,101 > 51건

--조회 (게시판)
--최신글을 먼저 ... 나중에 쓴 글이 먼저
--글번호 (순번)

--최신글순으로 보여 주세요
--select * from board order by boardnum desc;
--    (화면 출력)
--101   --51
--50    --50
--49    --49
--48...
--------------------------------------------------------------------------------
--개발자(sequence, rownum)
--rownum 의사컬럼: 실제 물리적으로 존재하는 컬럼이 아니고 논리적인 존재

--ROWNUM: 실제로 테이블에 존재하지 않는 컬럼(행 번호 부여)
--ROWID : 주소값(행이 실제로 저장되는 내부 주소값) -> 인덱스 만들어 질때 사용...

select empno, ename from emp;

select rownum as 순번, empno from emp;

--Top-n 쿼리
--테이블에서 조건에 맞는 상위 (TOP) 레코드에서 n개 추출...
--근거: 정렬(기준)

--ms-sql: select top 10 --> select top 10, * from emp order by sal desc

--oracle: rownum (의사컬럼): 기준이 필요하다
--1.정렬쿼리를 만든다
--2.쿼리에 순번을 부여하고 조건절 제시

select * from emp order by sal desc;

--1단계
select e.* from (select * from emp order by sal desc) e;

--2단계
select rownum, e.* from (select * from emp order by sal desc) e
where rownum <= 10;

/*
--실행 시점 (subquery를 사용하는 이유)
select rownum as n, empno, sal -- 2
from emp                       -- 1
order by sal desc;             -- 3
*/

select e.*
from (
        select empno, sal from emp
        order by sal desc
      ) e
where rownum < 5; --내부적인 처리 순서 때문에 크다는 표현은 (rownum 다시 만들어서)
--------------------------------------------------------------------------------
--게시판 (페이징 처리 원리)
--100건 (row)

--총 100건
--pagesize = 10 정의 (화면 한 페이지에 볼 수 있는 데이터 건수 10개)
--건수 / pagesize > page 개수 > 10
--pagecount => 10개

--1~100
/*
  1~10
    1page
  11~20
    2page
    ...
  91~100
    10page
*/
--------------------------------------------------------------------------------
--HR 계정으로...
select * from employees;

select *
from(
    select rownum num, e.*
    from(
        select * from employees order by employee_id asc
        ) e
    where rownum <= 50
    ) s
    where num > 40;

--1~10, 11~20, 21~30, 31~40, 41~50
--                           (5번 페이지)

select *
from(
    select rownum num, e.*
    from(
        select * from employees order by employee_id asc
        ) e
    where rownum <= 10
    ) s
    where num > 0;

--between A and B > where rownum between 10 and 20
--------------------------------------------------------------------------------
--SQL 1차 학습 END
```
