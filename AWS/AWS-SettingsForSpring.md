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

## Elastic IP가 존재할 경우, Route 53을 통해 도메인 연결
> http://wingsnote.com/57

## AWS EC2 ubuntu에서 tomcat 구동 시 느릴 경우 조치
```
> sudo vi /usr/share/tomcat8/bin/catalina.sh

catalina.sh을 열어서 '#!/bin/sh' 바로 아래에 옵션 추가
JAVA_OPTS="$JAVA_OPTS -Djava.security.egd=file:/dev/./urandom"
```
> http://www.hwangji.kr/sub/dev_leader/link/os/default.aspx?NHBBSID=NHBoardWebTip&NHBBSIDX=74 <br>
> http://lng1982.tistory.com/261 <br>
> http://mikelim.mintocean.com/entry/Ubuntu%EC%97%90%EC%84%9C-tomcat%EC%9D%B4-%EB%8A%90%EB%A6%AC%EA%B2%8C-%EB%A1%9C%EB%93%9C-%EB%90%A0-%EB%95%8C
