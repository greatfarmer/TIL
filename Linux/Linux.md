# Linux

## 빨리 찾기
- [자주쓰는 명령어](#자주쓰는-명령어)
- [권한 관리](#권한-관리)
- [심볼릭 링크](#심볼릭-링크)
- [링크](#링크)

## 자주쓰는 명령어
- `df -h` 용량 확인
- `du -hs` 해당 디렉토리 용량 확인
- `netstat -tnlp` 네트워크 상태 확인
- `ps -ef | grep <디렉토리명>` 프로세스 상태 확인
- `rm -rf ./*` 현재 위치한 경로상의 폴더 및 파일 모두 삭제
- `history` 쉘 명령어 수행 목록 조회
- `last` 최근 접속 기록 및 최근 재부팅 기록 확인
- `free` 메모리 확인
- `top` 시스템에서 프로세스 목록을 CPU 사용률이 높은 것부터 확인

## 권한 관리
```
$ ll
drwxrwxr-x 3 greatfarmer greatfarmer 4096 Feb 25 20:07 ./
drwxr-xr-x 5 greatfarmer greatfarmer 4096 Feb 25 20:05 ../
lrwxrwxrwx 1 greatfarmer greatfarmer   21 Feb 25 20:07 app -> /home/greatfarmer/application/
drwxrwxr-x 2 greatfarmer greatfarmer 4096 Feb 25 20:07 foo/
-rwxrw-r-- 1 greatfarmer greatfarmer    6 Feb 25 20:06 hello.txt
```
```
-rwxrw-r-- 1 greatfarmer greatfarmer    6 Feb 25 20:06 hello.txt
```
해석
- 파일(-)이며
- 첫번째(rwx) 소유자가 읽기, 쓰기, 실행 권한이 있다
- 두번째(rw-) 그룹이 읽기, 쓰기 권한이 있다
- 세번째(r--) 다른 사용자가 읽기 권한이 있다

첫번째 글자 의미
- `d` 디렉토리
- `l` 링크
- `-` 파일

이후 3글자씩 끊어서 관리
- 첫번째: 소유자 권한
- 두번째: 그룹 권한
- 세번째: 다른 사용자 권한

각 글자 의미
- `r` 읽기(read) 권한
- `w` 쓰기(write) 권한
- `x` 실행(execute) 권한

퍼미션 변경
- `chmod [변경될 퍼미션값] [변경할 대상]`
```
-rwxrw-r-- 1 greatfarmer greatfarmer    6 Feb 25 20:06 hello.txt
```
- 위의 권한을 -rwxr-xr-x로 변경하고 싶다
```
rwx r-x r-x를 이진수로 나타내면
111 101 101 -> 755
```
```
$ chmod 755 hello.txt
```
```
-rwxr-xr-x 1 greatfarmer greatfarmer    6 Feb 25 20:06 hello.txt
```

소유자 변경
- `chown [변경할 소유자]:[변경할 그룹] [변경할 대상]`
```
-rwxr-xr-x 1 greatfarmer greatfarmer    6 Feb 25 20:06 hello.txt
```
- 위의 소유자와 그룹을 queen으로 변경하고 싶다
```
$ chown queen:queen hello.txt
```
```
-rwxr-xr-x 1 queen       queen          6 Feb 25 20:06 hello.txt
```

## 심볼릭 링크
- `ln -s [원본 파일 또는 디렉토리] [심볼릭 링크 이름]`
```
$ ln -s /home/greatfarmer/application app
```
```
lrwxrwxrwx 1 greatfarmer greatfarmer   29 Feb 25 20:49 app -> /home/greatfarmer/application/
```

## 링크
- [제타위키 리눅스](https://zetawiki.com/wiki/분류:리눅스)
- [스왑파일(가상메모리) 만들기](http://gafani.tistory.com/entry/스왑파일가상메모리-만들기)
- [리눅스 프로세스 상태보기 (ps명령)](http://smile2x.tistory.com/entry/리눅스-프로세스-상태-보기ps명령)
- [crontab 사용법](https://jdm.kr/blog/2)
- SSMTP로 서버 상태를 메일로 받기
  - https://yellowtopaz.tistory.com/185
  - https://wiki.ubuntu-kr.org/index.php/SSMTP
  - [(참고) gmail의 SMTP 설정](http://devgwangpal.tistory.com/34)
    - `gmail` -> `환경 설정` -> `전달 및 pop/IMAP` -> `IMAP액세스` -> `IMAP 사용 설정`
      - https://support.google.com/mail/troubleshooter/1668960?hl=ko&rd=2#ts=1665018
    - `내 계정` -> `로그인 및 보안` -> `연결된 앱 및 사이트` -> `보안 수준이 낮은 앱 허용`
      - http://micropilot.tistory.com/category/Java%20Mail/Gmail%20Account
- [리눅스 서버 60초안에 상황파악하기](https://b.luavis.kr/server/linux-performance-analysis)
- [리눅스 LVM 생성](http://sgbit.tistory.com/13)
- [CentOs에 jdk설치 및 환경변수(JAVA_HOME)설정](https://blog.hanumoka.net/2018/04/30/centOs-20180430-centos-install-jdk/)
- [rpm 명령어로 패키지 설치 및 업그레이드](http://pchero21.com/?p=1424)
- [파이썬 paramiko라이브러리로 ssh접속](https://hakurei.tistory.com/222)
- [[UNIX / Linux] 권한 관리(chmod, chown, chgrp, umask)](https://eunguru.tistory.com/93)