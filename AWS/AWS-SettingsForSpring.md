# Spring 프로젝트의 AWS 설정

## 태웅이가 정리한 자료
```
data/AWS 발표자료.pptx
```

## EC2 생성 및 설정
> http://jojoldu.tistory.com/259

## Ubuntu 접속 후 java8, apache2, tomcat8 설치

> http://blog.moramcnt.com/?p=1061

## Ubuntu 환경에서 Maven으로 Spring 프로젝트를 Tomcat에 배포
> http://all-record.tistory.com/186

## Spring & AWS S3(using sdk for java) 연동
> https://docs.aws.amazon.com/ko_kr/AmazonS3/latest/dev/RetrievingObjectUsingJava.html <br>
> https://docs.aws.amazon.com/ko_kr/AmazonS3/latest/dev/UsingTheMPJavaAPI.html <br>
> https://shj7242.github.io/2017/12/28/Spring34/ <br>
> http://jojoldu.tistory.com/300 <br>
> https://docs.minio.io/docs/how-to-use-aws-sdk-for-java-with-minio-server <br>
> http://blog.woniper.net/218

## AWS S3 등 Access Key를 github에서 관리
[AWS 고객센터 원문]<br>
We see a lot of keys accidentally exposed through GitHub because it does not scrape code for sensitive information when posting to your repository. Fortunately AWS has created a tool to help developers using GitHub avoid this problem.<br><br>
If you use GitHub at all I recommend setting up our open source tool, git-secrets, to prevent users of your account from accidentally committing access keys. You can find the code and all documentation related to this tool here:
> https://github.com/awslabs/git-secrets

## Elastic IP가 존재할 경우, Route 53을 통해 도메인 연결
> http://wingsnote.com/57

## AWS EC2 ubuntu에서 tomcat 구동 시 느릴 경우 조치
첫번째 방법
```
> sudo vi /usr/share/tomcat8/bin/catalina.sh

catalina.sh을 열어서 '#!/bin/sh' 바로 아래에 옵션 추가
JAVA_OPTS="$JAVA_OPTS -Djava.security.egd=file:/dev/./urandom"
```
두번째 방법
```
> cat /proc/sys/kernel/random/entropy_avail

먼저 패키지를 설치하기 전에 다음 명령어를 통하여 현재 entropy를 확인한다.
결과치가 1000 이하일 경우 haveged를 설치할 것을 권장하고 있다.

> sudo apt-get install haveged

```
> http://www.hwangji.kr/sub/dev_leader/link/os/default.aspx?NHBBSID=NHBoardWebTip&NHBBSIDX=74 <br>
> http://lng1982.tistory.com/261 <br>
> http://mikelim.mintocean.com/entry/Ubuntu%EC%97%90%EC%84%9C-tomcat%EC%9D%B4-%EB%8A%90%EB%A6%AC%EA%B2%8C-%EB%A1%9C%EB%93%9C-%EB%90%A0-%EB%95%8C

## AWS EC2 ubuntu에서 tomcat 구동 시 handler processing failed; nested exception is java.lang.outofmemoryerror: java heap space 에러 조치
JVM heap memory 용량 부족 에러: ubuntu의 톰캣 설정에서 메모리 설정을 변경
```
> sudo vi /usr/share/tomcat8/bin/catalina.sh

catalina.sh을 열어서 '#!/bin/sh' 바로 아래에 옵션 추가
JAVA_OPTS="$JAVA_OPTS -Xms256m -Xmx1024m -XX:MaxPermSize=128m"
```
> http://mycup.tistory.com/215
> https://okky.kr/article/319932

## AWS EC2 ubuntu에서 tomcat 구동 시 한글처리
server.xml
```xml
<Connector port="8080" protocol="HTTP/1.1"
              connectionTimeout="20000"
              URIEncoding="UTF-8"
              useBodyEncodingForURl="true"
              redirectPort="8443" />
```
> http://kingko.tistory.com/entry/JDK
