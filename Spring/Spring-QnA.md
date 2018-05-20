# Spring Q&A

## Q. DTO 클래스의 Bean 설정해주지 않아 객체 생성이 되는 이유?
> http://yookeun.github.io/java/2014/07/04/spring-component-scan/

## Q. Maven Dependency 오류가 발생할 경우?
```
1. 아래 Problem탭에서 에러가 나는 Maven 이름을 확인한다.
2. 해당 Maven 폴더를 지우고 다시 Maven dependency를 다운받는다.
3. 해당 프로젝트 오른쪽 클릭 - [Maven] - [Update Project]를 완료한다.

위의 방법으로도 Maven에러가 해결되지 않을 시,
C:\Users\사용자명\.m2\repository 내의 폴더 및 파일을 백업 한 후 모두 삭제
그리고 다시 Maven을 다운받고 Update Project를 진행
```
