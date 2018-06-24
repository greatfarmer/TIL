# AWS RDS 설정

## AWS RDS의 mariaDB에서 select * from user;를 하면 table 'user' doesn't exist 에러 발생 시 (대소문자 구분 문제)

AWS RDS에서는 리눅스에 mariaDB를 설치해 주는데, 리눅스에서는 기본적으로 테이블 대소문자 구별을 한다.
따라서 강제적으로 대소문자 구분하지 않게 하기 위해서는 파라미터그룹에서
```lower_case_table_names```를 1로 설정해주어야 한다.

* 0일 경우 : 대소문자 구분함
* 1일 경우 : 대소문자 구분안함

> http://egloos.zum.com/forbis/v/3500411
<br> http://roqkffhwk.tistory.com/91

## AWS RDS MySQL 에서 데이터베이스 Super (root) 권한 행사하기

파라미터 그룹에서 ```log_bin_trust_function_creators```를 1로 설정해주어야 한다.

> http://luckyyowu.tistory.com/316

## AWS RDS MariaDB에 Command Prompt로 접속하기
MariaDB Command Prompt 실행 후
```
> mysql -h corin2mariadb.cat5tvbkgafa.ap-northeast-2.rds.amazonaws.com -P 3306 -u corin2 -p
```
이후, 비밀번호 입력하여 접속
