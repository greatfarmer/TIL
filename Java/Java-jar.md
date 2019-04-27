# 실행 가능한 jar파일 만들고 실행하기

## class파일 생성
```sh
[mac@greatfarmer ~]$ vi Bar.java
```

```java
public class Bar {
  public void print() {
    System.out.println("Bar");
  }
}
```

```sh
[mac@greatfarmer ~]$ vi Foo.java
```

```java
public class Foo {
  public static void main(String[] args) {
    System.out.println("Foo");
    Bar bar = new Bar();
    bar.print();
  }
}
```

```sh
[mac@greatfarmer ~]$ ls
Bar.java  Foo.java
[mac@greatfarmer ~]$ mkdir foo
[mac@greatfarmer ~]$ javac Foo.java -d foo/
[mac@greatfarmer ~]$ ls
Bar.java  Foo.java  foo
[mac@greatfarmer ~]$ cd foo
[mac@greatfarmer ~/foo]$ ls
Bar.class  Foo.class
[mac@greatfarmer ~/foo]$ cd ..
```

## manifest 생성
```sh
[mac@greatfarmer ~]$ vi mymanifest
```

```
Main-Class: Foo
```

## jar파일 생성
```sh
[mac@greatfarmer ~]$ jar cvfm classes.jar mymanifest -C foo/ .
Manifest를 추가함
추가하는 중: Foo.class(입력 = 449) (출력 = 308)(31%를 감소함)
추가하는 중: Bar.class(입력 = 373) (출력 = 264)(29%를 감소함)
```

## jar파일 실행
```sh
[mac@greatfarmer ~]$ java -jar classes.jar
Foo
Bar
```

## jar
```sh
[mac@greatfarmer ~]$ jar
사용법: jar {ctxui}[vfmn0PMe] [jar-file] [manifest-file] [entry-point] [-C dir] files ...
옵션:
    -c  새 아카이브를 생성합니다.
    -t  아카이브에 대한 목차를 나열합니다.
    -x  명명된(또는 모든) 파일을 아카이브에서 추출합니다.
    -u  기존 아카이브를 업데이트합니다.
    -v  표준 출력에 상세 정보 출력을 생성합니다.
    -f  아카이브 파일 이름을 지정합니다.
    -m  지정된 Manifest 파일의 Manifest 정보를 포함합니다.
    -n  새 아카이브를 생성한 후 Pack200 정규화를 수행합니다.
    -e  jar 실행 파일에 번들로 제공된 독립형 애플리케이션의
        애플리케이션 시작 지점을 지정합니다.
    -0  저장 전용: ZIP 압축을 사용하지 않습니다.
    -P  파일 이름에서 선행 '/'(절대 경로) 및 ".."(상위 디렉토리) 구성요소를 유지합니다.
    -M  항목에 대해 Manifest 파일을 생성하지 않습니다.
    -i  지정된 jar 파일에 대한 인덱스 정보를 생성합니다.
    -C  지정된 디렉토리로 변경하고 다음 파일을 포함합니다.
특정 파일이 디렉토리일 경우 순환적으로 처리됩니다.
Manifest 파일 이름, 아카이브 파일 이름 및 시작 지점 이름은
'm', 'f' 및 'e' 플래그와 동일한 순서로 지정됩니다.

예 1: classes.jar라는 아카이브에 두 클래스 파일을 아카이브하는 방법:
       jar cvf classes.jar Foo.class Bar.class
예 2: 기존 Manifest 파일 'mymanifest'를 사용하여
           foo/ 디렉토리의 모든 파일을 'classes.jar'로 아카이브하는 방법:
       jar cvfm classes.jar mymanifest -C foo/ .
```

출처: https://ra2kstar.tistory.com/125 [초보개발자 이야기.]