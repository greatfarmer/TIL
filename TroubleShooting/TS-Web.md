# TroubleShooting: Web

### 헤더 들롭다운 잘리는 현상 (2018.06.19)
문제
```
헤더에서 드롭다운이 잘려 보이는 현상
```

해결
```
css 속성에 overflow: hidden;을 작성한 것이 문제였다. 이를 지워줘야 드롭다운이 w잘리지 않는다.
```

### 크롬 스크롤 제어 (2018.06.20)
문제
```
스크롤을 채팅의 유저리스트div에서만 없애고 싶다
```

해결
```
스크롤 기능 가능하면서, 스크롤바를 안보이게 하는 방법
(원하는 요소에만 적용할 수 있도록, 전체를 하려면 ::-webkit-scrollbar만 쓰면됨)
css에 아래 내용 추가 (원하는 요소::-webkit-scrollbar)
```
```css
.sideBar::-webkit-scrollbar {
  display:none;
}
```
> 출처: https://m.blog.naver.com/PostView.nhn?blogId=fageapp&logNo=220392875038&proxyReferer=https%3A%2F%2Fwww.google.co.kr%2F"
