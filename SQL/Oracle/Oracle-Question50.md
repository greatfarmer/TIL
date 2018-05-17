# Oracle Question 50

## EMP table query
```sql
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
```

## Questions
```sql
--1> 부서테이블의 모든 데이터를 출력하라.


--2> EMP테이블에서 각 사원의 직업, 사원번호, 이름, 입사일을 출력하라.


--3> EMP테이블에서 직업을 출력하되, 각 항목(ROW)가 중복되지 않게
-- 출력하라.


--4> 급여가 2850 이상인 사원의 이름 및 급여를 출력하라.


--5> 사원번호가 7566인 사원의 이름 및 부서번호를 출력하라.


--6> 급여가 1500이상 ~ 2850이하의 범위에 속하지 않는 모든 사원의 이름
-- 및 급여를 출력하라.


--7> 1981년 2월 20일 ~ 1981년 5월 1일에 입사한 사원의 이름,직업 및
--입사일을 출력하라. 입사일을 기준으로 해서 오름차순으로 정렬하라.


--8> 10번 및 30번 부서에 속하는 모든 사원의 이름과 부서 번호를
-- 출력하되, 이름을 알파벳순으로 정렬하여 출력하라.

--9> 10번 및 30번 부서에 속하는 모든 사원 중 급여가 1500을 넘는
-- 사원의 이름 및 급여를 출력하라.
--(단 컬럼명을 각각 employee 및 Monthly Salary로 지정하시오)

--10> 관리자가 없는 모든 사원의 이름 및 직위를 출력하라.


--11> 커미션을 받는 모든 사원의 이름, 급여 및 커미션을 출력하되,
-- 급여를 기준으로 내림차순으로 정렬하여 출력하라.


--12> 이름의 세 번째 문자가 A인 모든 사원의 이름을 출력하라.

--13> 이름에 L이 두 번 들어가며 부서 30에 속해있는 사원의 이름을
--출력하라.


--14> 직업이 Clerk 또는 Analyst 이면서 급여가 1000,3000,5000 이
-- 아닌 모든 사원의 이름, 직업 및 급여를 출력하라.

--15> 사원번호, 이름, 급여 그리고 15%인상된 급여를 정수로 표시하되
--컬럼명을 New Salary로 지정하여 출력하라.


--16> 15번 문제와 동일한 데이타에서 급여 인상분(새 급여에서 이전
-- 급여를 뺀 값)을 추가해서 출력하라.(컬럼명은 Increase로 하라).



--18> 모든 사원의 이름(첫 글자는
-- 대문자로, 나머지 글자는 소문자로 표시) 및 이름 길이를 표시하는
-- 쿼리를 작성하고 컬럼 별칭은 적당히 넣어서 출력하라.


--19> 사원의 이름과 커미션을 출력하되, 커미션이 책정되지 않은
-- 사원의 커미션은 'no commission'으로 출력하라.


--20> 모든 사원의 이름,부서번호,부서이름을 표시하는 질의를 작성하라.


--21> 30번 부서에 속한 사원의 이름과 부서번호 그리고 부서이름을 출력하라.


--22> 30번 부서에 속한 사원들의 모든 직업과 부서위치를 출력하라.
--(단, 직업 목록이 중복되지 않게 하라.)


--23> 커미션이 책정되어 있는 모든 사원의 이름, 부서이름 및 위치를 출력하라.


--24> 이름에 A가 들어가는 모든 사원의 이름과 부서 이름을 출력하라.


--25> Dallas에서 근무하는 모든 사원의 이름, 직업, 부서번호 및 부서이름을
-- 출력하라.


--26> 사원이름 및 사원번호, 해당 관리자이름 및 관리자 번호를 출력하되,
-- 각 컬럼명을 employee,emp#,manager,mgr#으로 표시하여 출력하라.

--27> 모든 사원의 이름,직업,부서이름,급여 및 등급을 출력하라.


--28> Smith보다 늦게 입사한 사원의 이름 및 입사일을 출력하라.

--29> 자신의 관리자보다 먼저 입사한 모든 사원의 이름, 입사일,
-- 관리자의 이름, 관리자의 입사일을 출력하되 각각 컬럼명을
-- Employee,EmpHiredate, Manager,MgrHiredate로 표시하여
-- 출력하라.


--30> 모든 사원의 급여 최고액,최저액,총액 및 평균액을 출력하되
-- 각 컬럼명을 Maximum,Minimum,Sum,Average로 지정하여 출력하라.

--31> 각 직업별로 급여 최저액.최고액,총액 및 평균액을 출력하라.

--32> 직업이 동일한 사람 수를 직업과 같이 출력하라.

--33> 관리자의 수를 출력하되, 관리자 번호가 중복되지 않게하라.
-- 그리고, 컬럼명을 Number of Manager로 지정하여 출력하라.


--34> 최고 급여와 최저 급여의 차액을 출력하라.

--35> 관리자 번호 및 해당 관리자에 속한 사원들의 최저 급여를 출력하라.
-- 단, 관리자가 없는 사원 및 최저 급여가 1000 미만인 그룹은 제외시키고
-- 급여를 기준으로 출력 결과를 내림차순으로 정렬하라.


--36> 부서별로 부서이름, 부서위치, 사원 수 및 평균 급여를 출력하라.
-- 그리고 각각의 컬럼명을 부서명,위치,사원의 수,평균급여로 표시하라.


--37> Smith와 동일한 부서에 속한 모든 사원의 이름 및 입사일을 출력하라.
-- 단, Smith는 제외하고 출력하시오


--38> 자신의 급여가 평균 급여보다 많은 모든 사원의 사원 번호, 이름, 급여를
--    표시하는 질의를 작성하고 급여를 기준으로 결과를 내림차순으로 정렬하라.


--39> 이름에 T가 들어가는 사원의 속한 부서에서 근무하는 모든 사원의 사원번호
-- 및 이름을 출력하라.


--40> 부서위치가 Dallas인 모든 사원의 이름,부서번호 및 직위를 출력하라.


--41> KING에게 보고하는 모든 사원의 이름과 급여를 출력하라.


--42> Sales 부서의 모든 사원에 대한 부서번호, 이름 및 직위를 출력하라.


--43> 자신의 급여가 평균 급여보다 많고 이름에 T가 들어가는 사원과 동일한
-- 부서에 근무하는 모든 사원의 사원 번호, 이름 및 급여를 출력하라.


--44> 커미션을 받는 사원과 급여가 일치하는 사원의 이름,부서번호,급여를
-- 출력하라.


--45> Dallas에서 근무하는 사원과 직업이 일치하는 사원의 이름,부서이름,
--     및 급여를 출력하시오


--46> Scott과 동일한 급여 및 커미션을 받는 모든 사원의 이름, 입사일 및
-- 급여를 출력하시오


--47> 직업이 Clerk 인 사원들보다 더 많은 급여를 받는 사원의 사원번호,
-- 이름, 급여를 출력하되, 결과를 급여가 높은 순으로 정렬하라.


--48> 이름에 A가 들어가는 사원과 같은 직업을 가진 사원의 이름과
-- 월급, 부서번호를 출력하라.


--49> New  York 에서 근무하는 사원과 급여 및 커미션이 같은 사원의
-- 사원이름과 부서명을 출력하라.



--50> Dallas에서 근무하는 사원과 직업 및 관리자가 같은 사원의 사원번호,사원이름,
--    직업,월급,부서명,커미션을 출력하되 커미션이 책정되지 않은 사원은 NoCommission
--    으로 표시하고, 커미션의 컬럼명은 Comm으로 나오게 출력하시오.
--    단, 최고월급부터 출력되게 하시오

```

## Answers
```sql
--1> 부서테이블의 모든 데이터를 출력하라.
SELECT *
FROM EMP

--2> EMP테이블에서 각 사원의 직업, 사원번호, 이름, 입사일을 출력하라.
SELECT JOB, EMPNO, ENAME, HIREDATE
FROM EMP

--3> EMP테이블에서 직업을 출력하되, 각 항목(ROW)가 중복되지 않게 출력하라.
SELECT DISTINCT JOB
FROM EMP

--4> 급여가 2850 이상인 사원의 이름 및 급여를 표시하는 출력하라.
SELECT ENAME, SAL
FROM EMP
WHERE SAL >= 2850

--5> 사원번호가 7566인 사원의 이름 및 부서번호를 표시하는 출력하라.
SELECT ENAME, DEPTNO
FROM EMP
WHERE EMPNO=7566

--6> 급여가 1500이상 ~ 2850이하의 범위에 속하지 않는 모든 사원의 이름 및 급여를 출력하라.
SELECT ENAME, SAL
FROM EMP
WHERE SAL NOT BETWEEN 1500 AND 2850

--7> 1981년 2월 20일 ~ 1981년 5월 1일에 입사한 사원의 이름,직업 및 입사일을 출력하라. 입사일을 기준으로 해서 오름차순으로 정렬하라.
SELECT ENAME, JOB, HIREDATE
FROM EMP
WHERE HIREDATE BETWEEN '1981-02-20' AND '1981-05-01'
ORDER BY HIREDATE

--8> 10번 및 30번 부서에 속하는 모든 사원의 이름과 부서 번호를 출력하되, 이름을 알파벳순으로 정렬하여 출력하라.

SELECT ENAME, DEPTNO
FROM EMP
WHERE DEPTNO IN(10,30)
ORDER BY ENAME

--9> 10번 및 30번 부서에 속하는 모든 사원 중 급여가 1500을 넘는 사원의 이름 및 급여를 출력하라.
--(단 컬럼명을 각각 employee 및 Monthly Salary로 지정하시오)
SELECT ENAME AS 'Employee', SAL AS ' Monthly Salary'
FROM EMP
WHERE DEPTNO IN(10,30) AND SAL>1500

--10> 관리자가 없는 모든 사원의 이름 및 직위를 출력하라.
SELECT ENAME, JOB
FROM EMP
WHERE MGR IS NULL


--11> 커미션을 받는 모든 사원의 이름, 급여 및 커미션을 출력하되, 급여를 기준으로 내림차순으로 정렬하여 출력하라.
SELECT ENAME, SAL, COMM
FROM EMP
WHERE COMM IS NOT NULL
ORDER BY SAL DESC

--12> 이름의 세 번째 문자가 A인 모든 사원의 이름을 출력하라.
SELECT ENAME
FROM EMP
WHERE ENAME LIKE '__A%'

--13> 이름에 L이 두 번 들어가며 부서 30에 속해있는 사원의 이름을 출력하라.
SELECT ENAME
FROM EMP
WHERE ENAME LIKE '%L%L%' AND DEPTNO=30

--14> 직업이 Clerk 또는 Analyst 이면서 급여가 1000,3000,5000 이 아닌 모든 사원의 이름, 직업 및 급여를 출력하라.

SELECT ENAME, JOB, SAL
FROM EMP
WHERE JOB IN('CLERK','ANALYST') AND SAL not IN(1000,3000,5000)

--15> 사원번호, 이름, 급여 그리고 15%인상된 급여를 정수로 표시하되 컬럼명을 New Salary로 지정하여 출력하라.
SELECT EMPNO, ENAME, SAL,
           ROUND(SAL+(SAL*0.15),0) AS "New Salary"
FROM EMP

--16> 15번 문제와 동일한 데이타에서 급여 인상분(새 급여에서 이전 급여를 뺀 값)을 추가해서 출력하라.(컬럼명은 Increase로 하라).
SELECT EMPNO, ENAME, SAL, ROUND(SAL+(SAL*0.15) ) AS "New Salary",
  (ROUND(SAL+(SAL*0.15)))-SAL AS "Increase"
FROM EMP

--18> 모든 사원의 이름(첫 글자는 대문자로, 나머지 글자는 소문자로 표시) 및 이름 길이를 표시하는 쿼리를 작성하고 컬럼 별칭은 적당히 넣어서 출력하라.
SELECT UPPER(SUBSTR(ENAME,1,1)) || LOWER(SUBSTR(ENAME,2,12)) AS "NAME",
LENGTH(ENAME) AS "LENENAME"
FROM EMP


--19> 사원의 이름과 커미션을 출력하되, 커미션이 책정되지 않은 사원의 커미션은 'no commission'으로 출력하라.
SELECT ENAME, NVL(TO_CHAR(COMM),'no commission')
FROM EMP

--20> 모든 사원의 이름,부서번호,부서이름을 표시하는 질의를 작성하라.(DECODE)

SELECT empno, deptno , DECODE(deptno,10,'ACCOUNTING' ,
                                                    20,'RESEARCH',
                                                    30,'SALES',
                                                    40,'OPERATIONS') AS "dname"
FROM EMP;


SELECT E.ENAME, E.DEPTNO, D.DNAME
FROM EMP E, DEPT D
WHERE E.DEPTNO=D.DEPTNO

--21> 30번 부서에 속한 사원의 이름과 부서번호 그리고 부서이름을 출력하라.
SELECT E.ENAME, E.DEPTNO, D.DNAME
FROM EMP E, DEPT D
WHERE E.DEPTNO=D.DEPTNO AND E.DEPTNO=30

--22> 30번 부서에 속한 사원들의 모든 직업과 부서위치를 출력하라.
--(단, 직업 목록이 중복되지 않게 하라.)
SELECT DISTINCT E.JOB, D.LOC
FROM EMP E, DEPT D
WHERE E.DEPTNO=D.DEPTNO AND D.DEPTNO=30

--23> 커미션이 책정되어 있는 모든 사원의 이름, 부서이름 및 위치를 출력하라.
SELECT E.ENAME, D.DNAME, D.LOC
FROM EMP E, DEPT D
WHERE E.DEPTNO=D.DEPTNO AND COMM IS NOT NULL

--24> 이름에 A가 들어가는 모든 사원의 이름과 부서 이름을 출력하라.
SELECT E.ENAME, D.DNAME
FROM EMP E, DEPT D
WHERE E.DEPTNO=D.DEPTNO AND E.ENAME LIKE '%A%'

--25> Dallas에서 근무하는 모든 사원의 이름, 직업, 부서번호 및 부서이름을 출력하라.
SELECT E.ENAME, E.JOB, E.DEPTNO, D.DNAME
FROM EMP E, DEPT D
WHERE E.DEPTNO=D.DEPTNO AND D.LOC='Dallas'

--26> 사원이름 및 사원번호, 해당 관리자이름 및 관리자 번호를 출력하되, 각 컬럼명을 employee,emp#,manager,mgr#으로 표시하여 출력하라.
SELECT E.ENAME AS "EMPPLOYEE", E.EMPNO AS "EMP#",
M.ENAME AS "MANAGER", M.EMPNO AS "MGR#"
FROM EMP E, EMP M
WHERE E.MGR=M.EMPNO

--27> 모든 사원의 이름,직업,부서이름,급여 및 등급을 출력하라.
SELECT E.ENAME, E.JOB, D.DNAME, E.SAL, S.GRADE
FROM EMP E, DEPT D, SALGRADE S
WHERE E.DEPTNO=D.DEPTNO
AND E.SAL BETWEEN S.LOSAL AND S.HISAL

--28> Smith보다 늦게 입사한 사원의 이름 및 입사일을 출력하라.
SELECT ENAME, HIREDATE
FROM EMP
WHERE HIREDATE>(SELECT HIREDATE
   FROM EMP
   WHERE ENAME='SMITH')

--29> 자신의 관리자보다 먼저 입사한 모든 사원의 이름, 입사일, 관리자의 이름, 관리자의 입사일을 출력하되 각각 컬럼명을 Employee,EmpHiredate, Manager,MgrHiredate로 표시하여 출력하라.
SELECT E.ENAME AS "Employee", E.HIREDATE AS "EmpHIredate",
M.ENAME AS "Manager",
 M.HIREDATE AS "MgrHiredate"
FROM EMP E, EMP M
WHERE E.MGR=M.EMPNO AND E.HIREDATE<M.HIREDATE

--30> 모든 사원의 급여 최고액,최저액,총액 및 평균액을 출력하되 각 컬럼명을 Maximum, Minimum, Sum, Average로 지정하여 출력하라.
SELECT MAX(SAL) AS "Maximum", MIN(SAL) AS "Minimum",
 SUM(SAL) AS "Sum", AVG(SAL) AS "Average"
FROM EMP

--31> 각 직업별로 급여 최저액.최고액,총액 및 평균액을 출력하라.
SELECT JOB, MAX(SAL), MIN(SAL), SUM(SAL), AVG(SAL)
FROM EMP
GROUP BY JOB

--32> 직업이 동일한 사람 수를 직업과 같이 출력하라.
SELECT JOB, COUNT(JOB)
FROM EMP
GROUP BY JOB

--33> 관리자의 수를 출력하되, 관리자 번호가 중복되지 않게하라. 그리고, 컬럼명을 Number of Manager로 지정하여 출력하라.
SELECT COUNT(DISTINCT(MGR)) AS "Number of Manager"
FROM EMP

--34> 최고 급여와 최저 급여의 차액을 출력하라.
SELECT MAX(SAL)-MIN(SAL) FROM EMP

--35> 관리자 번호 및 해당 관리자에 속한 사원들의 최저 급여를 출력하라. 단, 관리자가 없는 사원 및 최저 급여가 1000 미만인 그룹은 제외시키고 급여를 기준으로 출력 결과를 내림차순으로 정렬하라.
SELECT MGR, MIN(SAL)
FROM EMP
WHERE MGR IS NOT NULL
GROUP BY MGR
HAVING MIN(SAL)>=1000
ORDER BY MIN(SAL)

--36> 부서별로 부서이름, 부서위치, 사원 수 및 평균 급여를 출력하라. 그리고 각각의 컬럼명을 부서명,위치,사원의 수,평균급여로 표시하라.
SELECT E.DEPTNO AS "부서명", D.DNAME AS "부서이름", D.LOC AS "위치",
 COUNT(E.EMPNO) AS "사원의 수", AVG(E.SAL) AS "평균급여"
FROM EMP E, DEPT D
WHERE E.DEPTNO=D.DEPTNO
GROUP BY E.DEPTNO, D.DNAME, D.LOC

--37> Smith와 동일한 부서에 속한 모든 사원의 이름 및 입사일을 출력하라. 단, Smith는 제외하고 출력하시오
SELECT ENAME, TO_CHAR(HIREDATE,'YYYY-MM-DD')
FROM EMP
WHERE DEPTNO IN(SELECT DEPTNO
   FROM EMP
   WHERE ENAME='SMITH')
AND ENAME!='SMITH'

--38> 자신의 급여가 평균 급여보다 많은 모든 사원의 사원 번호, 이름, 급여를 표시하는 질의를 작성하고 급여를 기준으로 결과를 내림차순으로 정렬하라.
SELECT EMPNO, ENAME, SAL
FROM EMP
WHERE SAL>(SELECT AVG(SAL)
  FROM EMP)
ORDER BY SAL DESC

--39> 이름에 T가 들어가는 사원의 속한 부서에서 근무하는 모든 사원의 사원번호 및 이름을 출력하라.
SELECT EMPNO, ENAME
FROM EMP
WHERE DEPTNO IN(SELECT DEPTNO
   FROM EMP
   WHERE ENAME LIKE '%T%')

--40> 부서위치가 Dallas인 모든 사원의 이름,부서번호 및 직위를 출력하라.
SELECT E.ENAME, E.DEPTNO, E.JOB
FROM EMP E, DEPT D
WHERE E.DEPTNO=D.DEPTNO
AND D.LOC='DALLAS'

--41> KING에게 보고하는 모든 사원의 이름과 급여를 출력하라.
SELECT ENAME, SAL
FROM EMP
WHERE MGR=(SELECT EMPNO
  FROM EMP
  WHERE ENAME='KING')

--42> Sales 부서의 모든 사원에 대한 부서번호, 이름 및 직위를 출력하라.
SELECT E.DEPTNO, E.ENAME, E.JOB
FROM EMP E, DEPT D
WHERE E.DEPTNO=D.DEPTNO
 AND D.DNAME='SALES'

SELECT E.DEPTNO, E.ENAME, E.JOB
FROM EMP E inner join DEPT D on E.DEPTNO=D.DEPTNO
where  D.DNAME='SALES'

--43> 자신의 급여가 평균 급여보다 많고 이름에 T가 들어가는 사원과 동일한 부서에 근무하는 모든 사원의 사원 번호, 이름 및 급여를 출력하라.
SELECT EMPNO, ENAME, SAL
FROM EMP
WHERE SAL > (SELECT AVG(SAL)
  FROM EMP)
AND DEPTNO IN(SELECT DEPTNO
  FROM EMP
  WHERE ENAME LIKE '%T%')

--44> 커미션을 받는 사원과 급여가 일치하는 사원의 이름,부서번호,급여를 출력하라.
SELECT ENAME, DEPTNO, SAL
FROM EMP
WHERE SAL IN(SELECT SAL
  FROM EMP
  WHERE COMM IS NOT NULL)

--45> Dallas에서 근무하는 사원과 직업이 일치하는 사원의 이름,부서이름, 및 급여를 출력하시오
SELECT E.ENAME, D.DNAME, E.SAL
FROM EMP E, DEPT D
WHERE E.DEPTNO=D.DEPTNO
AND E.JOB IN(SELECT E.JOB
  FROM EMP E, DEPT D
  WHERE E.DEPTNO=D.DEPTNO
  AND D.LOC='DALLAS')

--46> Scott과 동일한 급여 및 커미션을 받는 모든 사원의 이름, 입사일 및 급여를 출력하시오
SELECT ENAME, HIREDATE, SAL
FROM EMP
WHERE SAL=(SELECT SAL
  FROM EMP
  WHERE ENAME='SCOTT')
AND NVL(COMM,0)=(SELECT NVL(COMM,0)
  FROM EMP
  WHERE ENAME='SCOTT')

--47> 직업이 Clerk 인 사원들보다 더 많은 급여를 받는 사원의 사원번호, 이름, 급여를 출력하되, 결과를 급여가 높은 순으로 정렬하라.
SELECT EMPNO, ENAME, SAL
FROM EMP
WHERE SAL>ALL(SELECT SAL
  FROM EMP
  WHERE JOB='CLERK') --결국 최대값과 비교 any 최소값과 비교
ORDER BY SAL DESC

--48> 이름에 A가 들어가는 사원과 같은 직업을 가진 사원의 이름과 월급, 부서번호를 출력하라.
SELECT ENAME, SAL, DEPTNO
FROM EMP
WHERE JOB IN(SELECT JOB
  FROM EMP
  WHERE ENAME LIKE '%A%')

--49> New  York 에서 근무하는 사원과 급여 및 커미션이 같은 사원의 사원이름과 부서명을 출력하라.
select * from emp join dept
on emp.deptno = dept.deptno
and dept.loc='NEW YORK';

SELECT E.ENAME, D.DNAME
FROM EMP E, DEPT D
WHERE
e.deptno= d.deptno
AND E.SAL IN(
          SELECT E.SAL
                FROM EMP E, DEPT D
                WHERE E.DEPTNO=D.DEPTNO AND LOC='NEW YORK'
                  )
AND NVL(COMM,0) IN(
               SELECT NVL(COMM,0)
               FROM EMP E, DEPT D
               WHERE E.DEPTNO=D.DEPTNO
                              AND LOC='NEW YORK');

--50> Dallas에서 근무하는 사원과 직업 및 관리자가 같은 사원의 사원번호,사원이름, 직업,월급,부서명,커미션을 출력하되 커미션이 책정되지 않은 사원은 NoCommission으로 표시하고, 커미션의 컬럼명은 Comm으로 나오게 출력하시오. (단, 최고월급부터 출력되게 하시오)
SELECT E.EMPNO, E.ENAME, E.JOB, E.SAL, D.DNAME,
NVL((TO_CHAR(E.COMM)),'NoCommision') AS "COMM"
FROM EMP E, DEPT D
WHERE E.DEPTNO=D.DEPTNO
AND JOB IN(SELECT JOB
  FROM EMP E, DEPT D
  WHERE E.DEPTNO=D.DEPTNO AND LOC='DALLAS')
AND MGR IN(SELECT MGR
  FROM EMP E, DEPT D
  WHERE E.DEPTNO=D.DEPTNO AND LOC='DALLAS')
```
