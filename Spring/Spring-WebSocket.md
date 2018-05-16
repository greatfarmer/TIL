# Spirng WebSocket

## 정의
* WebSocket이 기존의 일반 TCP Socket과 다른 점은 최초 접속이 일반 http request를 통해 handshaking과정을 통해 이루어 진다는 점이다.
* http request를 그대로 사용하기 때문에 기존의 80, 443 포트로 접속을 하므로 추가로 방화벽을 열지 않고도 양방향 통신이 가능하고, http 규격인 CORS적용이나 인증등의 과정을 기존과 동일하게 가저갈 수 있는것이 장점이다.
