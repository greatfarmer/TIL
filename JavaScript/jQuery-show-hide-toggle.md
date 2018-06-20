# jQuery show() / hide() / toggle()
> http://jdm.kr/blog/27

## show() & hide() Example
```javascript
$("#tagID").show(); // display 속성을 block 으로 바꾼다.
$("#tagID").hide(); // display 속성을 none 으로 바꾼다.
```

## show() / hide() Syntax
```javascript
$(selector).show(speed,callback);
$(selector).hide(speed,callback);
```
```
* selector - 태그 ID 값 또는 선택할 노드들의 셀렉터 구문을 넣어줍니다.
* speed(optional) - "slow", "fast", 또는 밀리세컨드의 숫자를 넣어주면 보여주거나 감출 속도를 정할 수 있습니다.
* callback(optional) - 콜백 함수를 설정하면 show()/hide() 함수 완료 후 실행됩니다.
```

## show() / hide() Complex Example
```javascript
function toggle_layer() {
	if($("#layer").css("display") == "none"){
		$("#layer").show();
	}else{
		$("#layer").hide();
	}
}
```
위와 같은 함수를 만들땐 여러가지 복합적으로 처리를 할 때 가끔 사용하면 좋겠죠. 하지만 단순한 on/off 기능이 필요하다면 toggle() 함수를 써봅시다.

## toggle() Example
```javascript
$("#tagID").toggle(); // show -> hide , hide -> show
```
toggle()을 사용하게 되면 이전 상태에 따라 현재 상태를 반대로 바꿔줍니다.

## toggle() Syntax
```
$(selector).toggle(speed,callback);
```
```
* selector - 태그 ID 값 또는 선택할 노드들의 셀렉터 구문을 넣어줍니다.
* speed(optional) - "slow", "fast", 또는 밀리세컨드의 숫자를 넣어주면 보여주거나 감출 속도를 정할 수 있습니다.
* callback(optional) - 콜백 함수를 설정하면 toggle() 함수 완료 후 실행됩니다.
```
