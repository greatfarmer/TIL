# TroubleShooting: MySQL

### 2018-06-01 ~ 2018-06-30일 사이에 USER 테이블의 JOINDATE에 따른 카운터값을 통계를 내고자 한다.
* USER테이블의 해당 날짜가 없으면 추가해준다. (해당 기간의 날짜를 테이블로 만들고 left join으로 해당 카운터값을 넣어 준다.)
* 해당 날짜에 count값이 null이면 0으로 치환

#### 테이블
```sql
CREATE TABLE `USER` (
	`USERID`      VARCHAR(50)  NOT NULL COMMENT '아이디', -- 아이디
	`USERNAME`    VARCHAR(40)  NOT NULL COMMENT '닉네임', -- 닉네임
	`PASSWORD`    VARCHAR(255) NOT NULL COMMENT '비밀번호', -- 비밀번호
	`ENABLED`     INT          NOT NULL COMMENT '사용여부', -- 사용여부
	`USERPROFILE` VARCHAR(255) NULL     COMMENT '프로필사진', -- 프로필사진
	`JOINDATE`    DATETIME     NOT NULL COMMENT '가입일', -- 가입일
	`GRADENUM`    VARCHAR(10)  NOT NULL COMMENT '등급넘버', -- 등급넘버
	`ISDELETED`   INT          NOT NULL COMMENT '탈퇴여부' -- 탈퇴여부
);
```

#### 존재하는 JOINDATE의 카운터 (날짜별 USER수)
```sql
SELECT
    DATE_FORMAT(joindate,'%Y-%m-%d') as date, COUNT(*) as count
FROM user
WHERE joindate between '2018-06-15' and '2018-06-31'
GROUP BY
    DATE_FORMAT(joindate,'%Y-%m-%d')
ORDER BY
    DATE_FORMAT(joindate,'%Y-%m-%d');
```

#### 어떤 기간 동안 날짜 컬럼 만들어 주기
```sql
select * from
(select adddate('1970-01-01',t4*10000 + t3*1000 + t2*100 + t1*10 + t0) gen_date from
 (select 0 t0 union select 1 union select 2 union select 3 union select 4 union select 5 union select 6 union select 7 union select 8 union select 9) t0,
 (select 0 t1 union select 1 union select 2 union select 3 union select 4 union select 5 union select 6 union select 7 union select 8 union select 9) t1,
 (select 0 t2 union select 1 union select 2 union select 3 union select 4 union select 5 union select 6 union select 7 union select 8 union select 9) t2,
 (select 0 t3 union select 1 union select 2 union select 3 union select 4 union select 5 union select 6 union select 7 union select 8 union select 9) t3,
 (select 0 t4 union select 1 union select 2 union select 3 union select 4 union select 5 union select 6 union select 7 union select 8 union select 9) t4) v
where gen_date between '2018-06-01' and '2018-06-30';
```

#### 최종 쿼리
```sql
select v.gen_date as date, ifnull(u.count, 0) as count from
(select adddate('1970-01-01',t4*10000 + t3*1000 + t2*100 + t1*10 + t0) gen_date from
 (select 0 t0 union select 1 union select 2 union select 3 union select 4 union select 5 union select 6 union select 7 union select 8 union select 9) t0,
 (select 0 t1 union select 1 union select 2 union select 3 union select 4 union select 5 union select 6 union select 7 union select 8 union select 9) t1,
 (select 0 t2 union select 1 union select 2 union select 3 union select 4 union select 5 union select 6 union select 7 union select 8 union select 9) t2,
 (select 0 t3 union select 1 union select 2 union select 3 union select 4 union select 5 union select 6 union select 7 union select 8 union select 9) t3,
 (select 0 t4 union select 1 union select 2 union select 3 union select 4 union select 5 union select 6 union select 7 union select 8 union select 9) t4) v
left join
 (SELECT DATE_FORMAT(joindate,'%Y-%m-%d') as date, COUNT(*) as count
 FROM user
 GROUP BY DATE_FORMAT(joindate,'%Y-%m-%d')
 ORDER BY DATE_FORMAT(joindate,'%Y-%m-%d')) u
on v.gen_date = u.date
where gen_date between '2018-06-01' and '2018-06-30';
```
