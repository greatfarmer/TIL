# TroubleShooting: Node.js

### node.js 버전 문제 [2018-09-03]
#### 문제
$ yarn global add eslint 실행시 아래 에러 발생
```
error eslint@5.4.0: The engine "node" is incompatible with this module. Expected version "^6.14.0 || ^8.10.0 || >=9.10.0".
```

#### 해결
node.js 버전확인 명령어: ```$ node -v```

문제 발생 시 node 버전: ```v.8.9.4```

해당 에러에서

"^6.14.0 || ^8.10.0 || >=9.10.0"

의미는 6.14.x, 8.10.x, 9.10 이상버전에서 사용 가능하다는 것이므로, node를 최신 LTS 버전으로 설치하여 해결

```v.8.11.4``` 버전으로 업그레이드 후 문제 해결 완료
