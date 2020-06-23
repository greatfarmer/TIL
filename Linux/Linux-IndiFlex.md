# 리눅스 시니어코딩
- [영상](https://www.youtube.com/watch?v=MHzxhoBmCwA&list=PLEOnZ6GeucBVj0V5JFQx_6XBbZrrynzMh)
- [슬라이드](https://docs.google.com/presentation/d/1CrOcTTrRRHlredMRwie9WKSo7ChIF4bRylvUxhinRYU/edit?usp=sharing)

## 도커
- 슬라이드 참고

## 리눅스 01 - 리눅스 기초
- 슬라이드 참고

## 리눅스 02 - vi 에디터
- 도커 이름
  - docker ps -a --format '{{.Names}}'
- 도커 이름, 상태
  - docker ps -a --format '{{.Names}} || {{.Status}}'
- 도커 이름, 상태, 이미지
  - docker ps -a --format '{{.Names}} || {{.Status}} || {{.Image}}'
- 한글 설정
  - locale
  - locale -a
  - LC_ALL=ko_KR.utf8 bash
  - export LANGUAGE=ko
- vim 명령어
```
i: 바로 앞에 입력모드
a: 바로 뒤에 입력모드
H/M/L: 화면의 제일 위, 중간, 아래로 커서 이동
Ctrl+F: PageDn
Ctrl+B 또는 Ctrl+U: PageUp
$: End key
^: Home key
u: Undo(Ctrl+z) -> undo 했던걸 다시 취소: Ctrl+r
x: delete
X: backspace
dw: 한단어 삭제
cw: 한단어 수정
r: change 1 char
A: insert mode at line end 라인끝에서 입력모드 시작
o: 다음줄에 입력모드 시작
O: 윗줄에 입력모드 시작
dd: 한줄 지우기
yy: 한줄 복사
p: 붙여넣기
v: 블럭지정
Shift+d: 현재 커서위치 뒷부분 삭제
```

colon 명령
```
:라인번호 해당 라인번호로 이동
:$ 끝줄로 이동 (cf. 첫줄은 :1)
:w 저장
:q 나가기
:wq 저장하고 나가기
:q! 저장하지 않고 그냥 나가기
:! 커맨드 라인으로 잠깐 나가기 (enter시 복귀)
:!명령 커맨드라인에서 명령을 실행(ex. :!ls -al  ls -al 결과를 잠깐 보여줌)
:set nu 라인번호 보기
:set nonu 라인번호 숨기기
:%s/찾을문자/바꿀문자/ig
```

## 리눅스 03 - python, telnet, putty, git 설치하기
- ll p*
  - p로 시작하는 것만 보겠다

## 리눅스 04 - 도커에서 리눅스 컨테이너 구동 팁
- 슬라이드 참고

## 리눅스 05 - Shell Script & Cron
- 용량확인
  - df -k
  - df -h
- 디렉토리, 파일 용량 확인
  - du -sk workspace
  - du -sh workspace
  - du -sk ps.txt
  - du -sh ps.txt
- !! history 바로 전 명령어
- !d histroy에서 d로 시작하는 명령어
- ~/.vimrc vim의 환경설정
  - 슬라이드 참고
- 쉘에서 현재 폴더의 파일, 디렉토리 목록 for문
  - $ for i in `ls`; do echo $i; done
- alias
  - alias s1='~/s1.sh'
- shell script
  - if
    - `gt` greater than >
    - `lt` less than <
    - `eq` equal =
    - `ne` not equal !=
    - `le` less equal <=
    - `ge` great and equal >=