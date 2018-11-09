# jsoup: Java HTML Parser
> https://jsoup.org/

`jsoup` is a Java library for working with real-world HTML. It provides a very convenient API for extracting and manipulating data, using the best of DOM, CSS, and jquery-like methods.

## 기본
- [Java HTML parser, Jsoup로 원하는 값 얻어내기](http://partnerjun.tistory.com/search/jsoup)
- [jsoup : 자바 HTML 파서(Java HTML Parser)](https://offbyone.tistory.com/116)

## 상황별 정리
### jsop, JSON parser로 HTML내 json 추출
- https://stackoverflow.com/questions/33707679/how-to-get-this-script-from-html-with-jsoup-in-android-programming

### response가 json타입인 경우 처리 (ajax)
```java
// HTML Document가 아니므로 Response의 컨텐트 타입을 무시한다.
.ignoreContentType(true)
```
- https://stackoverflow.com/questions/7133118/jsoup-requesting-json-response
- http://partnerjun.tistory.com/51
