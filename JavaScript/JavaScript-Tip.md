# [JavaScript] Tip

## 랜덤 색상(hex)
```javascript
function getRandomColor() {
  return '#' + Math.floor(Math.random() * 16777215).toString(16);
  // 16777215 -> (hex) -> FFFFFF
}
```

## keyboard events: keyCode
  - http://noritersand.tistory.com/224
